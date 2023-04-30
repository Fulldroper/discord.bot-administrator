<template>
  <div ref="context" class="flex flex-col w-52 z-auto absolute p-2 bg-base1 rounded-lg">
    <ContextMenu v-for="tab in contextmenuOptions.tabs" @selected="selected" :key="tab.name" :tab="tab" />
  </div>
</template>

<script>
import ContextMenu from './ContextMenu.vue'
export default {
  props: {
    contextmenuOptions: Object,
  },
  created() {
    this.$watch('contextmenuOptions', () => {
      this.chengePos()
    })
  },
  methods: {
    chengePos(){
      this.$refs.context.style.top = `${this.contextmenuOptions.y }px`
      this.$refs.context.style.left = `${this.contextmenuOptions.x  }px`
    },
    selected(e){
      this.$emit('selected', { id: this.contextmenuOptions.id, name: e.name, fn: e.fn })
    }
  },
  mounted() {
    this.chengePos()
  },
}
</script>