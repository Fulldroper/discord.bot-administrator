<template>
  <div @click="onClick" @mousedown="mousedown" @mouseup="mouseup" @contextmenu="botContextMenu" ref="img" class="w-full mb-2 hover:bg-base3 bg-base2 rounded-full hover:rounded-mdPlus cursor-pointer bg-center bg-no-repeat bg-contain" :alt="username"></div>
</template>

<script>
export default {
  data() {
    return {
    }
  },
  props: {
    avatar: String,
    username: String,
    id: String,
  },
  mounted () {
    this.$refs.img.style.height = this.$refs.img.offsetWidth + "px";
    this.avatar && (this.$refs.img.style.backgroundImage = `url("https://cdn.discordapp.com/avatars/${this.id}/${this.avatar}?size=1024")`)
  },
  methods: {
    onClick () {
      this.$emit("click", this.id)
    },
    mouseup({target}) {
      target.classList.remove("hold")
    },
    mousedown({target}) {
      target.classList.add("hold")
    },
    botContextMenu(e) {
      e.id = this.id
      this.$emit("botContextMenu", e)
    }
  }
}
</script>

<style scoped>
  div:hover::before {
    content: attr(alt);
    background-color: var(--color-base-1);
    /* position: relative; */
    position: absolute;
    transform: translateY(25%);
    left: 68px;
    /* left: 125%; */
    /* left: 20px; */
    display: block;
    z-index: 10;
    border-radius: 6px;
    padding: 6px;
    width: max-content, 200px;
    font-size: 12px;
    font-weight: bold;
    color: white;
    /* top: 25%; */
  }
  .hold {
    transform: translateY(2.5px);
  }
</style>