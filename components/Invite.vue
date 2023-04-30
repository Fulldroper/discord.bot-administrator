<template>
  <tr class="p-3 text-xl text-left">
    <th class="text-baseblue">{{ invite.inviter.username }}#{{ invite.inviter.discriminator }}</th>
    <th class="ml-2 text-gray-500">{{ invite.channel.name }} . {{ invite.channel.type }}</th>
    <th class="ml-2 text-gray-500">{{ invite.code }}</th>
    <th class="ml-2 text-gray-500">{{ invite.uses }} / {{ invite.max_uses > 0 ? invite.max_uses : "∞" }}</th>
    <th class="ml-2 text-gray-500">{{ invite.max_age > 0 ? dateFormat() : "∞" }}</th>
  </tr>
</template>

<script>
export default {
  props: {
    invite: Object,
  }, 
  methods: {
    dateFormat(){
      return new Date(new Date(this.invite.expires_at).getTime() + this.invite.max_age).toLocaleString('en-GB', {
        day: '2-digit',
        month: '2-digit',
        year: 'numeric',
        hour: '2-digit',
        minute: '2-digit',
        second: '2-digit',
      });
    }
  }
}
// { "code": "25tGVXYY", "type": 0, "expires_at": "2023-04-18T12:45:39+00:00", "channel": { "id": "977532189585600593", "name": "🔔・roles", "type": 0 }, "inviter": { "id": "312658820952227841", "username": "papichtrapich","avatar": "ae1e77ff0249bc916e23c89ce6a05f8a", "discriminator": "2587"}, "uses": 0, "max_uses": 0, "max_age": 604800, "temporary": false, "created_at": "2023-04-11T12:45:39.110000+00:00" } 
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