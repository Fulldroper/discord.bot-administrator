<template>
  <div class="z-2 w-max h-max absolute flex flex-col items-end right-4 bottom-4">
    <div v-for="alert in alerts" :key="alert.name" class="p-4 mt-4 bg-base1 rounded-lg w-max h-max ">{{ alert.name }}</div>
  </div>
</template>


<script>
export default {
  props: {
    alerts: Array,
  }
}
</script>