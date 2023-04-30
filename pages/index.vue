<template>
  <div class="flex flex-row flex-nowrap w-screen h-screen" @contextmenu.prevent @click="clickAny">
    <Navbar @addBot="addBotModule" @botContextMenu="botContextMenu" :bots="bots" :logo="logo" @browseBot="guildsList" />
    <ContextMenuList v-if="this.contextmenu" @selected="contextmenuSelect" :contextmenuOptions="contextmenuOptions" />
    <GuildsList v-if="content === 'guildsList'" :guilds="guilds" @selectGuild="selectGuildPage" />
    <GuildPage v-else-if="content === 'guildPage' && selectGuild" :guild="selectGuild" :gateway="gateway" :selectBot="selectBot" />
    <EmptyContent v-else />
    <ModalToken v-if="botAddModule" @cancle="botAddModule = false" @confirm="addingBotFromModule" />
    <ModalMassMessage v-if="isMassMessageModal" @cancle="isMassMessageModal = false" @confirm="massMessage" />
    <LeaveServerModal v-if="guilds.length > 0 && isLeaveServerModal" :guilds="guilds" @cancle="isLeaveServerModal = false" @confirm="LeaveServer" />
    <AlertsModal v-if="alerts.length > 0" :alerts="alerts" />
  </div>
</template>

<script>
import Navbar from '../components/Navbar.vue';
import ModalToken from '../components/ModalToken.vue';
import EmptyContent from '../components/EmptyContent.vue';
import GuildsList from '../components/GuildsList.vue';
import GuildPage from '../components/GuildPage.vue';
import ContextMenuList from '../components/ContextMenuList.vue';
import ModalMassMessage from '../components/ModalMassMessage.vue';
import AlertsModal from '../components/AlertsModal';
import LeaveServerModal from '../components/LeaveServerModal';
export default {
  data() {
    const tokens = JSON.parse(
      localStorage?.getItem('tokens') || `[]`
    )
    const api = {
      gateway: "discord.com",
      version: '9'
    }
    return {
      logo: "https://cdn.discordapp.com/attachments/539138991031844864/1094553273740046347/636e0a6cc3c481a15a141738_icon_clyde_white_RGB.png",
      tokens,
      bots: [],
      guilds: [],
      botAddModule: false,
      api,
      isMassMessageModal: false,
      isLeaveServerModal: false,
      contextmenu:false,
      contextmenuOptions: false,
      selectBot: false,
      alertsTimer: false,
      alerts: [],
      selectGuild: false,
      content: false,
      gateway: {
        getme: `https://${api.gateway}/api/v${api.version}/users/@me`,
        myguilds: `https://${api.gateway}/api/v${api.version}/users/@me/guilds`,
        createDM: `https://${api.gateway}/api/v${api.version}/users/@me/channels`,
        guild: id => `https://${api.gateway}/api/v${api.version}/guilds/${id}?with_counts=true`,
        guildChannels: id => `https://${api.gateway}/api/v${api.version}/guilds/${id}/channels`,
        guildMembers: (id, limit = 100, skip = 0) => `https://${api.gateway}/api/v${api.version}/guilds/${id}/members?limit=${limit}&after=${skip}`,
        guildMember: (gid, uid) => `https://${api.gateway}/api/v${api.version}/guilds/${gid}/members/${uid}`,
        guildBans: id => `https://${api.gateway}/api/v${api.version}/guilds/${id}/bans`,
        guildRoles: id => `https://${api.gateway}/api/v${api.version}/guilds/${id}/roles`,
        guildPrune: id => `https://${api.gateway}/api/v${api.version}/guilds/${id}/prune`,
        guildInvites: id => `https://${api.gateway}/api/v${api.version}/guilds/${id}/invites`,
        sendMessage: id => `https://${api.gateway}/api/v${api.version}/channels/${id}/messages`,
        leaveGuild: id => `https://${api.gateway}/api/v${api.version}/users/@me/guilds/${id}`,
        // based on https://github.com/Fulldroper/cors-proxy#readme
        proxy: "http://cors.fd-c.cf/"
      }
    }
  },
  methods: {
    onError(...error){
      console.error(...error)
    },
    addBotModule() {
      this.botAddModule = true
    },
    addingBotFromModule(token) {
      if(this.tokens.includes(token)) return
      this.addBot(token)
        .then(res => (
          this.bots.push(res),
          this.tokens.push(token),
          this.saveTokens(),
          this.botAddModule = false
        ))
        .catch(this.error)
    },
    saveTokens() {
      localStorage?.setItem("tokens", JSON.stringify(this.tokens));
    },
    async guildsList(bot_id) {
      this.selectBot = this.bots.filter(({id}) => id === bot_id)[0];

      this.$axios.get(this.gateway.myguilds,{
        headers: {
          Authorization: `Bot ${this.selectBot.token}`
        }
      })
      .then(({data}) => {
        this.guilds = data
        this.content = "guildsList"
        return data
      })
      .catch(this.error)
    },
    async selectGuildPage(guild_id) {
      this.$axios.get(this.gateway.proxy, {
        headers: {
          Authorization: `Bot ${this.selectBot.token}`,
          "cors-url": this.gateway.guild(guild_id),
          "cors-ignore": "user-agent"
        }
      })
      .then(({data}) => {
        this.selectGuild = data
        this.content = "guildPage"
      })
      .catch(this.error)
    },
    async addBot(token) {
      return new Promise((resolve, reject) => {
        this.$axios.get(this.gateway.getme,{
          headers: {
            Authorization: `Bot ${token}`
          }
        })
        .then(({data}) => resolve({id: data.id, username: data.username, avatar: data.avatar, token}))
        .catch(reject)
      })
    },
    clickAny() {
      this.contextmenu = false
    },
    botContextMenu(e) {
      this.contextmenu = true
      this.contextmenuOptions = {
        x: e.clientX,
        y: e.clientY,
        id: e.id,
        tabs: [
          { name:'Dumping', color: 'hover:bg-baseblue text-baseblue' },
          { name:'Leave server', color: 'hover:bg-baseblue text-baseblue', fn: "leaveServerModal" },
          { name:'Owners brodcast', color: 'hover:bg-baseblue text-baseblue', fn: "massMessageModal" },
          { name:'Remove bot', color: 'hover:bg-basered text-basered', fn: "removeBot"}
        ]
      }
    },
    leaveServerModal({id}) {
      this.guildsList(id).then(() => {
        this.isLeaveServerModal = true
      }).catch(console.error)
    },
    massMessageModal({id}) {
      this.guildsList(id).then(() => {
        this.isMassMessageModal = true
      }).catch(console.error)
    },
    async LeaveServer(value) {
      await this.$axios.delete(this.gateway.proxy, {
        headers: {
          Authorization: `Bot ${this.selectBot.token}`,
          "cors-url": this.gateway.leaveGuild(value),
          "cors-ignore": "user-agent"
        }
      })
      this.guilds = this.guilds.filter(guild => guild.id!== value)
      this.isLeaveServerModal = false
    },
    createAlertsTimer() {
      this.alertsTimer = setInterval(() => {
        if (this.alerts.length > 0) {
          this.alerts.shift()
        }
      }, 8000)
    },
    async massMessage(value){
      this.isMassMessageModal = false
      this.guilds.forEach(async ({id}) => {
        this.$axios.get(this.gateway.proxy, {
        headers: {
          Authorization: `Bot ${this.selectBot.token}`,
          "cors-url": this.gateway.guild(id),
          "cors-ignore": "user-agent"
        }
        })
        .then(({data}) => {
          return this.$axios.post(this.gateway.proxy,{
              recipient_id: data.owner_id
            }, {
            headers: {
              Authorization: `Bot ${this.selectBot.token}`,
              "cors-url": this.gateway.createDM,
              "cors-ignore": "user-agent"
            }
          })
        })
        .then(({data}) => {
          const owner = `${data.recipients[0].username}#${data.recipients[0].discriminator}`
          this.$axios.post(this.gateway.proxy,{
              content: value,
              "embeds":[],
              "components": [],
              "sticker_ids": []
            }, {
            headers: {
              Authorization: `Bot ${this.selectBot.token}`,
              "cors-url": this.gateway.sendMessage(data.id),
              "cors-ignore": "user-agent"
            }
          })
          .then(e => {
            this.alerts.push({name: `${e.data.author.username}#${e.data.author.discriminator} send to ${owner}`});
          })
          .catch(console.error)
        })        
        .catch(console.error)
      })
    },
    removeBot(e) {
      this.bots.forEach((bot) => {
        if(bot.id === e.id) {
          this.tokens = this.tokens.filter(t => t !== bot.token)
          this.bots = this.bots.filter(bot => bot.id !== e.id)

          this.saveTokens()

          return
        }
      })
    },
    contextmenuSelect(e) {
      if (this[e.fn]) {
        this[e.fn](e)
      } else {
        console.log(e);
      }
    }
  },
  mounted() {
    this.createAlertsTimer()
    for (const token of this.tokens) {
      this.addBot(token).then(res => this.bots.push(res)).catch(this.error)
    }
  }
}
</script>

<style>
:root {
  --color-base-1:#1e2124;
  --color-base-1-op:#1e2124d8;
  --color-base-2:#282b30;
  --color-base-3:#36393e;
  --color-base-4:#424549;
  --color-base-blue:#7289da;
}

* {
  margin: 0;
  padding: 0;
  outline:none;
}

body {
  background-color: #282b30;
  color: #fff;
}

</style>