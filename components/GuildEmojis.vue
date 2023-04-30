<template>
  <details class="w-full px-8 pt-8">
    <summary class="bg-base3 p-2.5 rounded-lg mb-4 cursor-pointer">Emojis</summary>
    <p class="flex flex-wrap p4 justify-center bg-base1 rounded-lg">
      <Emoji v-for="emoji in emojis" :key="emoji.id" :emoji="emoji" />
    </p>
  </details>
</template>

<script>
import Emoji from './Emoji.vue';
export default {
  props: {
    emojis: {
      type: Array,
      required: true
    }
  }
}
</script>