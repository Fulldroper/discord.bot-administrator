<template>
  <tr class="p-3 text-xl text-left">
    <th class="text-baseblue">{{ ban.user.username }}#{{ ban.user.discriminator }}</th>
    <th class="ml-2 text-gray-500">{{ ban.reason }}</th>
  </tr>
</template>

<script>
export default {
  props: {
    ban: Object,
  }
}
</script>

<style scoped>
  th {
    border-right-color: var(--color-base-1);
    border-right-style: solid;
    border-right-width: 2px;
    border-left-color: var(--color-base-1);
    border-left-style: solid;
    border-left-width: 2px;
    padding: 8px;
  }
</style>