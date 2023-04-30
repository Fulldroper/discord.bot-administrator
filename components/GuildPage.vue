<template>
  <div class="flex flex-col w-full min-h-fit">
    <div ref="banner" class="bg-baseblue min-h-96 h-full rounded-b-large bage flex flex-row justify-center p-4 bg-center bg-no-repeat bg-cover border-baseblue border-solid">
      <div ref="ico" class="w-52 h-52 rounded-lg bg-base1op bg-center bg-no-repeat bg-cover bg-baseblue mr-4"></div>
      <div class="flex flex-col max-w-md bg-base1op rounded-lg p-5 h-max">
        <div class="text-baseblue text-2xl font-bold">{{ guild.name }}</div>
        <div class="mt-4">{{ guild.description }}</div>
        <div class="flex flex-row justify-between mt-4">
          <div>Region: {{ guild.region }}</div>
          <div class="mx-3">Locate: {{ guild.preferred_locale }}</div>
          <div>Members: {{ guild.approximate_member_count }}</div>
        </div>
      </div>
    </div>
    <div class="bg-base2 h-full overflow-y-auto justify-center">
      <div class="w-full px-8 pt-8 flex flex-row justify-center font-bold">
        <div class="p-2 m-2 bg-base3 hover:bg-base1 rounded-lg cursor-pointer" @click="page = 'home'">Home</div>
        <div class="p-2 m-2 bg-base3 hover:bg-base1 rounded-lg cursor-pointer" @click="showChannels">Channels</div>
        <div class="p-2 m-2 bg-base3 hover:bg-base1 rounded-lg cursor-pointer" @click="showRoles">Roles</div>
        <div class="p-2 m-2 bg-base3 hover:bg-base1 rounded-lg cursor-pointer" @click="showMembers">Members</div>
        <div class="p-2 m-2 bg-base3 hover:bg-base1 rounded-lg cursor-pointer" @click="showInvites">Invites</div>
        <div class="p-2 m-2 bg-base3 hover:bg-base1 rounded-lg cursor-pointer" @click="showBans">Bans</div>
      </div>
      <GuildMembers v-if="page === 'members'" :members="members" @loadMore="moreUsersLoad" />
      <GuildRoles v-if="page === 'roles'" :roles="roles" />
      <GuildChannels v-if="page === 'channels'" :channels="channels" />
      <GuildBans v-if="page === 'bans'" :bans="bans" />
      <GuildInvites v-if="page === 'invites'" :invites="invites" />
      <div v-if="page === 'home'">
        <GuildOwner v-if="this.owner" :member="owner" />
        <GuildEmojis v-if="guild?.emojis?.length > 0" :emojis="guild.emojis" />
        <GuildStickers v-if="guild?.stickers?.length > 0" :stickers="guild.stickers" />
      </div>
    </div>
  </div>
</template>

<script>
import GuildEmojis from './GuildEmojis.vue'
import GuildStickers from './GuildStickers.vue'
import GuildOwner from './GuildOwner.vue'
import GuildChannels from './GuildChannels.vue'
import GuildRoles from './GuildRoles.vue'
import GuildMembers from './GuildMembers.vue'
import GuildBans from './GuildBans.vue'
import GuildInvites from './GuildInvites.vue'
export default {
  data() {
    return {
      page: 'home',
      owner: false,
      members: false,
      roles: false,
      channels: false,
      bans: false,
      invites: false,
    }
  },
  methods: {
    async getOwner(gid, uid) {
      this.$axios.get(this.gateway.proxy,{
        headers: {
          Authorization: `Bot ${this.selectBot.token}`,
          "cors-url": this.gateway.guildMember(gid, uid),
          "cors-ignore": "user-agent"
        }
      })
      .then(({data}) => {
        this.owner = { ...data.user, roles: data.roles}
      })
      .catch(console.error)
    },
    async showRoles() {
      this.$axios.get(this.gateway.proxy,{
        headers: {
          Authorization: `Bot ${this.selectBot.token}`,
          "cors-url": this.gateway.guildRoles(this.guild.id),
          "cors-ignore": "user-agent"
        }
      })
     .then(({data}) => {
      this.roles = data
      this.page = "roles"
     })
    },
    async showChannels() {
      this.$axios.get(this.gateway.proxy,{
        headers: {
          Authorization: `Bot ${this.selectBot.token}`,
          "cors-url": this.gateway.guildChannels(this.guild.id),
          "cors-ignore": "user-agent"
        }
      }).then(({data}) => {
      this.channels = data
      this.page = "channels"
     })
    },
    async showMembers() {
      this.$axios.get(this.gateway.proxy,{
        headers: {
          Authorization: `Bot ${this.selectBot.token}`,
          "cors-url": this.gateway.guildMembers(this.guild.id),
          "cors-ignore": "user-agent"
        }
      }).then(({data}) => {
      this.members = data
      this.page = "members"
     })
    },
    async showInvites() {
      this.$axios.get(this.gateway.proxy,{
        headers: {
          Authorization: `Bot ${this.selectBot.token}`,
          "cors-url": this.gateway.guildInvites(this.guild.id),
          "cors-ignore": "user-agent"
        }
      }).then(({data}) => {
      this.invites = data
      console.log(this.invites)
      this.page = "invites"
     })
    },
    async showBans() {
      this.$axios.get(this.gateway.proxy,{
        headers: {
          Authorization: `Bot ${this.selectBot.token}`,
          "cors-url": this.gateway.guildBans(this.guild.id),
          "cors-ignore": "user-agent"
        }
      }).then(({data}) => {
        if (data.length > 0) {
          this.bans = data
          this.page = "bans"
        }
     })
    },
    async moreUsersLoad() {
      this.$axios.get(this.gateway.proxy,{
        headers: {
          Authorization: `Bot ${this.selectBot.token}`,
          "cors-url": this.gateway.guildMembers(this.guild.id, 100, this.members.at(-1).user.id),
          "cors-ignore": "user-agent"
        }
      }).then(({data}) => {
      this.members.push(...data)
     })
    }
  },
  props: {
    guild: Object,
    gateway: Object,
    selectBot: Object
  },
  mounted() {
    this.getOwner(this.guild.id, this.guild.owner_id)
    this.guild.icon && (this.$refs.ico.style.backgroundImage = `url("https://cdn.discordapp.com/icons/${this.guild.id}/${this.guild.icon}?size=1024")`)
    this.guild.banner && (this.$refs.banner.style.backgroundImage = `url("https://cdn.discordapp.com/banners/${this.guild.id}/${this.guild.banner}?size=1024")`)
  }
}
// id,  system_channel_id,  safety_alerts_channel_id, public_updates_channel_id
</script>

<style>
  .bage {
    max-height: 25%;
  }
</style>