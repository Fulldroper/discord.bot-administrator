<template>
    <table class="mt-8 px-8 py-8 w-full flex flex-col items-center">
    <tbody>
      <tr class="bg-base1 text-2xl text-basegreen max-w-5xl rounded-large mb-4">
        <th class="p-1">User</th>
        <th>Channel</th>
        <th>Code</th>
        <th>Uses</th>
        <th>Time</th>
      </tr>
      <Invite v-if="invites" v-for="invite in invites" :key="invite.code" :invite="invite" />
    </tbody>
  </table>
</template>

<script>
import Invite from "./Invite.vue"
export default {
  props: {
    invites: Array
  }
}
</script>