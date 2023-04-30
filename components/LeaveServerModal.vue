<template>
  <div class="z-2 w-screen h-screen absolute op flex items-center justify-center">
    <div class="h-max p-6 bg-base2 rounded-lg max-w-md w-full">
      <h2 class="text-white text-center text-2xl font-bold">Select server</h2>
      <select ref="guild" class="bg-base2 rounded-lg my-4 w-full">
        <option v-for="guild in guilds" :value="guild.id">
          {{guild.name}}
        </option>
      </select>
      <div class="flex flex-row justify-around text-white text-lg font-bold p-3 items-center">
        <div @click="cancle" class="text-basered p-2 cursor-pointer">Cancle</div>
        <div @click="confirm" @mousedown="mousedown" @mouseup="mouseup" class="bg-basegreen p-2 rounded-lg cursor-pointer text-center w-36">Leave</div>
      </div>
    </div>
  </div>
</template>

<style scoped>
  .op {
    background-color: var(--color-base-1-op);
  }
  input {
    outline: none !important;
  }
  .hold {
    transform: translateY(2.5px);
  }
</style>

<script>
export default {
  props: {
    guilds: Array,
  },
  methods: {
    cancle() {
      this.$emit('cancle')
    },
    confirm() {
      if (this.$refs.guild.value) {
        this.$emit('confirm', this.$refs.guild.value)
        this.$refs.guild.value = ""
      }
    },
    mouseup({target}) {
      target.classList.remove("hold")
    },
    mousedown({target}) {
      target.classList.add("hold")
    },
  }
}
</script>