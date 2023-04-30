<template>
  <div class="flex items-center flex-col w-full px-8 pt-8">
    <div class="text-center text-2xl mb-4 font-bold">Server owner</div>
    <div class="flex flex-row bg-base1 rounded-lg w-full chmaxw">
      <div ref="ico" class="w-52 h-52 rounded-lg bg-center bg-no-repeat bg-cover bg-baseblue mr-4"></div>
      <div class="flex flex-col justify-between w-fit">
        <div class="font-bold text-2xl py-4 pr-4 text-baseblue">{{ member.username }}#{{ member.discriminator }}</div>
        <div>{ member.roles }</div>
        <div class="p-2 text-center bg-basegreen cursor-pointer rounded-lg my-4 mr-4 hover:opacity-80">Send message</div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    member: Object,
  },
  mounted() { 
    this.member.avatar && (this.$refs.ico.style.backgroundImage = `url("https://cdn.discordapp.com/avatars/${this.member.id}/${this.member.avatar}?size=1024")`);
  }
}
</script>

<style> 
  .chmaxw {
    max-width: 460px;
  }
</style>