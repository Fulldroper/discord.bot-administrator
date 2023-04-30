<template>
  <div class="flex flex-wrap p-4 flex-col justify-center mt-8 px-8 w-max bg-baseblue rounded-tr-lg ">
    <Channel v-if="channels" v-for="channel in channels" :key="channel.id" :channel="channel" />
  </div>
</template>

<script>
import Channel from './Channel.vue';
export default {
  props: {
    channels: Array
  }
}
</script>