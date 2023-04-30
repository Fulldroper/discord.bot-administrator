<template>
  <div class="flex flex-wrap p4 justify-center mt-8 px-8">
    <GuildMember v-for="member in members" :key="member.id" :member="member" />
    <div v-if="members.length % 100 === 0" @click="loadMore" class="p-2 text-center bg-baseblue cursor-pointer rounded-lg my-4 mr-4 hover:opacity-80">Load more</div>
  </div>
</template>

<script>
import GuildMember from './GuildMember.vue';
export default {
  props: {
    members: Array,
  },
  methods: {
    loadMore() {
      this.$emit('loadMore');
    },
  }
}
</script>