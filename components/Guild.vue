<template>
  <div class="w-60 h-max bg-base1 rounded-lg p-4 cursor-pointer hover:bg-base3 up m-5" @click="onClick">
    <div ref="ico" class="fix-h w-full h-60 rounded-lg bg-center bg-no-repeat bg-cover bg-baseblue mb-4"></div>
    <h2 class="text-white text-center text-2xl font-bold">{{guild.name}}</h2>
  </div>
</template>

<script>
export default {
  props: {
    guild: Object,
  },
  mounted () {
    this.guild?.icon && (this.$refs.ico.style.backgroundImage = `url("https://cdn.discordapp.com/icons/${this.guild.id}/${this.guild.icon}?size=1024")`)
  },
  methods: {
    onClick () {
      this.$emit('click', this.guild.id)
    },
  },
}
</script>

<style scoped>
  .fix-h {
    height: 208px;
  }
  .up:hover {
    transform: translateY(-10px);
  }
</style>