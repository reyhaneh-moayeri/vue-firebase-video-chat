<template>
  <div class="nav flex justify-between align-center bg-red-500 p-4 text-white">
    <div>
      <router-link to="/rooms" class="font-bold text-lg" v-if="user">Rooms</router-link>
      <router-link to="/" class="font-bold text-xl mx-4">Video Chat</router-link>
    </div>
    <div class="justify-self-end">
      <router-link to="/login" v-if="!user">Login</router-link>
      <router-link class="mx-3" to="/register" v-if="!user">Regiter</router-link>
      <button @click="$emit('signout')" v-if="user">log out</button>
    </div>
  </div>
</template>
<script>
export default {
  name: 'Nav',
  props: ['user']
}
</script>
