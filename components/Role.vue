<template>
  <div class="flex flex-row items-center p-2 m-2">
    <div ref="ico" class="bg-center bg-no-repeat bg-contain bg-base2 w-4 h-4 mr-2"></div>
    <div ref="coloredText">{{ role.name }}</div>
  </div>
</template>

<script>
export default {
  props: {
    role: Object,
  },
  mounted() {
    this.role?.icon && (this.$refs.ico.style.backgroundImage = `url("https://cdn.discordapp.com/role-icons/${this.role.id}/${this.role.icon}?size=1024")`)
    this.role?.color && (this.$refs.coloredText.style.color = `#${this.role?.color?.toString(16)}`)
  }
}
// {{ role.name }}, {{ role.icon }}, {{ role.color }}
// https://cdn.discordapp.com/role-icons/499880560974626817/360b8a6be663923c268bf236c15995b9.webp?size=20&quality=lossless
// { "id": "454674083574185984", "name": "Адміністратор", "description": null, "permissions": "40149353758719", "position": 81, "color": 15577661, "hoist": true, "managed": false, "mentionable": true, "icon": "148b8b8ff6b25481edf1f9668a331bc2", "unicode_emoji": null, "flags": 0 }
</script>