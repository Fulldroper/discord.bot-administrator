<template>
  <table class="mt-8 px-8 py-8 w-full flex flex-col items-center">
    <tbody>
      <tr class="bg-base1 text-2xl text-basered max-w-5xl rounded-large mb-4">
        <th class="p-1">User</th>
        <th>Reason</th>
      </tr>
      <GuildBan v-for="ban in bans" :key="ban.user.id" :ban="ban" />
    </tbody>
  </table>
</template>

<script>
import GuildBan from './GuildBan.vue';
export default {
  props: {
    bans: Array,
  }
}
</script>