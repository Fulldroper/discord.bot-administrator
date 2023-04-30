<template>
  <div class="flex flex-wrap p4 justify-center mt-8 px-8">
    <Role v-for="role in roles" :key="role.id" :role="role" />
  </div>
</template>

<script>
import Role from './Role.vue';
export default {
  props: {
    roles: Array
  }
}
</script>