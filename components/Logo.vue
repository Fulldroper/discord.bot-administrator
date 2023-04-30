<template>
  <div ref="img" class="w-full mb-2 rounded-full bg-base3 bg-center bg-no-repeat bg-contain" alt="logo"></div>
</template>

<script>
export default {
  data() {
    return {
    }
  },
  props: {
    url: String
  },
  mounted () {
    this.$refs.img.style.backgroundImage = `url("./favicon.svg")`
    this.$refs.img.style.height = this.$refs.img.offsetWidth + "px";
  }
}
</script>

<style scoped>
  div {
    background-size: 60%;
  }
</style>