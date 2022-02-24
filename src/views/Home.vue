<template>
  <div class="home text-center container mx-auto">
    <p v-if="user" class="mt-3">
      welcome <span class="text-blue-600">{{ user.displayName }}</span>
    </p>
    <h1>Start a video chat</h1>
    <img class="w-32 mt-3 mx-auto" src="http://pixelprowess.com/i/powship.svg" alt="POW Ship" />
    <router-link
      v-if="user"
      to="/rooms"
      class="bg-red-500 text-white hover:bg-red-700 font-bold rounded-md px-4 py-2"
      >Rooms</router-link
    >
  </div>
</template>
<script>
// @ is an alias to /src

export default {
  name: 'Home',
  props: ['user']
}
</script>
