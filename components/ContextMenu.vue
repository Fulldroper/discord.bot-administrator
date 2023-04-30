<template>
  <div ref="color" @click="onClick" class="colormix hover:bg-baseblue p-1 rounded-md text-gray-500 hover:text-white cursor-pointer">
    {{ tab.name }}
  </div>
</template>

<script>
export default {
  props: {
    tab: Object,
  },
  methods: {
    onClick() {
      this.$emit("selected", {
        name: this.tab.name,
        fn: this.tab.fn
      })
    },
  }
}
</script>