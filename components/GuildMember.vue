<template>
  <div class="bg-base1 rounded-mdPlus m-4 items-center p-4 flex flex-col w-72">
    <div class="flex flex-row w-full">
    <div ref="ico" class="ico rounded-full bg-center bg-no-repeat bg-cover bg-baseblue mr-4"></div>
    <div class="flex flex-col justify-between w-fit">
      <div class="font-bold title py-2 pr-2 text-baseblue">{{ member.user.username }}#{{ member.user.discriminator }}</div>
      <div class="p-2 w-max bg-baseblue cursor-pointer rounded-lg my-4 mr-4 hover:opacity-80">
        <svg x="0" y="0" class="icon-2xnN2Y" aria-hidden="true" viewBox="0 0 24 24" fill="none"><path fill="currentColor" d="M4.79805 3C3.80445 3 2.99805 3.8055 2.99805 4.8V15.6C2.99805 16.5936 3.80445 17.4 4.79805 17.4H7.49805V21L11.098 17.4H19.198C20.1925 17.4 20.998 16.5936 20.998 15.6V4.8C20.998 3.8055 20.1925 3 19.198 3H4.79805Z"></path></svg>
      </div>
    </div>
  </div>
  <div v-if="member.nick" class="sub">nick: {{ member.nick }}</div>
  <div class="sub">joined at: {{ member.joined_at }}</div>
  </div>
  
</template>

<script>
export default {
  props: {
    member: Object,
  },
  mounted() { 
    this.member.user.avatar && (this.$refs.ico.style.backgroundImage = `url("https://cdn.discordapp.com/avatars/${this.member.user.id}/${this.member.user.avatar}?size=1024")`);
  }
}
// { "avatar": null, "communication_disabled_until": null, "flags": 0, "is_pending": false, "joined_at": "2022-06-24T09:24:24.185000+00:00", "nick": "Ztracer | Паша", "pending": false, "premium_since": null, "roles": [ "1012447249122459848", "1012635646810144808", "997566794871091242", "997566834284961964", "1050150675285295184", "630081346168946738", "977532383668621332", "997566832120705044", "999774045262336093" ], "user": { "id": "90026725194366976", "username": "Ztracer", "global_name": null, "display_name": null, "avatar": "3c799a81fd43b8ab7d59dfe3cbb38047", "discriminator": "6018", "public_flags": 640, "avatar_decoration": null }, "mute": false, "deaf": false } 
</script>

<style>
  .title{
    font-size: 12px;
  }
  .sub{
    font-size: 12px;
  }
  svg{
    width: 15px;
  }
  .ico{
    height: 85px;
    width: 85px;
  }
</style>