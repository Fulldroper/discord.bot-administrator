<template>
  <div class="bg-base1 w-16 p-2 h-screen max-h-screen overflow-x-auto overflow-y-hidden">
    <Logo :url=logo />
    <BotIcon @click="browseBot" @botContextMenu="botContextMenu" v-for="bot in bots" :key="bot.id" :id="bot.id" :username="bot.username" :avatar="bot.avatar" />
    <Addtoken @click="addBot" />
  </div>
</template>

<script>
import BotIcon from './BotIcon.vue';
import Addtoken from './Addtoken.vue';
import Logo from "./Logo.vue"

export default {
  methods: {
    addBot() {
      this.$emit("addBot");
    },
    browseBot(e) {
      this.$emit("browseBot", e);
    },
    botContextMenu(e) {
      this.$emit("botContextMenu", e)
    }
  },
  props: {
    bots: Array,
    logo: String,
  }
}
</script>