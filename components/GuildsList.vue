<template>
  <div class="h-screen flex p-10 min-w-0 flex-wrap overflow-y-auto justify-center w-fit">
    <Guild v-for="guild in guilds" :key="guild.id" :guild="guild" @click="onClick" />
  </div>
</template>

<script>
import Guild from './Guild.vue';
export default {
  props: {
    guilds: Array
  },
  methods:{
    onClick(guild){
      this.$emit('selectGuild', guild);
    }
  }
}
</script>