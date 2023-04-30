<template>
  <div class="z-2 w-screen h-screen absolute op flex items-center justify-center">
    <div class="h-max p-6 bg-base2 rounded-lg max-w-md w-full">
      <h2 class="text-white text-center text-2xl font-bold">Enter bot token here</h2>
      <ol class="text-gray-400 list-decimal p-4">
        <li>Go to the Discord Developer Portal website at <a class="text-gray-300" target="_blank" href="https://discord.com/developers/applications">https://discord.com/developers/applications</a>.</li>
        <li>Click the "New Application" button in the top-right corner of the page.</li>
        <li>Enter a name for your application and click the "Create" button.</li>
        <li>Click on the "Bot" tab on the left-hand side of the screen.</li>
        <li>Click the "Add Bot" button and confirm your action by clicking "Yes, do it!"</li>
        <li>Under the "TOKEN" section, click the "Copy" button to copy the bot token to your clipboard.</li>
      </ol>
      <input ref="token" class="bg-base2 rounded-lg my-4 w-full" type="password" placeholder="token">
      <h6 class="text-gray-500">
        Note: Be sure to keep your bot token secure, as anyone with access to it can control your bot and perform actions on your behalf.
      </h6>
      <div class="flex flex-row justify-around text-white text-lg font-bold p-3 items-center">
        <div @click="cancle" class="text-basered p-2 cursor-pointer">Cancle</div>
        <div @click="confirm" @mousedown="mousedown" @mouseup="mouseup" class="bg-basegreen p-2 rounded-lg cursor-pointer text-center w-36">Save token</div>
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
  methods: {
    cancle() {
      this.$emit('cancle')
    },
    confirm() {
      if (this.$refs.token.value) {
        this.$emit('confirm', this.$refs.token.value)
        this.$refs.token.value = ""
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