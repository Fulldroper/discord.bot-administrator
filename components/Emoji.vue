<template>
  <div>
    <div ref="ico" class="bg-center bg-no-repeat bg-contain bg-base2 mb-4 w-14 h-14 rounded-lg m-2"></div>
  </div>
</template>

<script>

export default {
  props: {
    emoji: {
      type: Object,
      required: true
    }
  },
  mounted() {
    this.$refs.ico.style.backgroundImage = `url("https://cdn.discordapp.com/emojis/${this.emoji.id}?size=1024")`
  }
}
</script>