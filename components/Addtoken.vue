<template>
  <div @click="onClick" @mousedown="mousedown" @mouseup="mouseup" ref="img" class="w-full mb-2 bg-base3 hover:rounded-mdPlus hover:bg-basegreen cursor-pointer rounded-full flex justify-center items-center" alt="Add bot">
    <svg class="w-7 h-7" viewBox="0 0 24 24"><path class="fill-basegreen" d="M20 11.1111H12.8889V4H11.1111V11.1111H4V12.8889H11.1111V20H12.8889V12.8889H20V11.1111Z"></path></svg>
  </div>
</template>

<style scoped>
  div:hover::after {
    content: attr(alt);
    background-color: var(--color-base-1);
    position: absolute;
    left: 68px;
    display: block;
    z-index: auto;
    border-radius: 6px;
    padding: 6px;
    width: max-content;
    font-size: 12px;
    font-weight: bold;
    color: white;
  }
  div:hover > svg > path {
    fill: white;
  }
  .hold {
    transform: translateY(2.5px);
  }
</style>

<script>
export default {
  methods: {
    onClick() {
      this.$emit("click");
    },
    mouseup() {
      this.$refs.img.classList.remove("hold")
    },
    mousedown() {
      this.$refs.img.classList.add("hold")
    }
  },
  mounted() {
    this.$refs.img.style.height = this.$refs.img.offsetWidth + "px";
  }
}
</script>