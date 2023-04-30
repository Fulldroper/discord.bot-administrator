<template>
  <details class="w-full px-8 pt-8">
    <summary class="bg-base3 p-2.5 rounded-lg mb-4 cursor-pointer">Stickers</summary>
    <p class="flex flex-wrap p-4 justify-center bg-base1 rounded-lg">
      <Sticker v-for="sticker in stickers" :key="sticker.id" :sticker="sticker" />
    </p>
  </details>
</template>

<script>
import Sticker from "./Sticker.vue"
export default {
  props: {
    stickers: {
      type: Array,
      required: true
    }
  }
}
</script>