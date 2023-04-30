<template>
  <div>
    <div ref="ico" class="bg-center bg-no-repeat bg-contain bg-base2 mb-4 w-32 h-32 rounded-lg m-2"></div>
  </div>
</template>

<script>

export default {
  props: {
    sticker: {
      type: Object,
      required: true
    }
  },
  mounted() {
    this.$refs.ico.style.backgroundImage = `url("https://media.discordapp.net/stickers/${this.sticker.id}?size=1024")`
  }
}
</script>