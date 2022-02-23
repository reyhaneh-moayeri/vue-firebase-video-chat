<template>
  <div class="home text-center container mx-auto">
    <p v-if="user" class="mt-3">
      welcome <span class="text-blue-600">{{ user }}</span>
    </p>
    <h1 class="mt-8 text-5xl px-6 font-extrabold text-red-500 tracking-tight">
      Start a video chat
    </h1>
    <img class="w-32 mt-3 mx-auto" src="http://pixelprowess.com/i/powship.svg" alt="POW Ship" />
  </div>
</template>
<script>
// @ is an alias to /src
import db from '../db.js'
import { collection, getDocs } from 'firebase/firestore'
export default {
  name: 'Home',
  components: {},
  data() {
    return {
      user: null
    }
  },
  methods: {
    async getUser() {
      const user = await collection(db, 'users')
      const userSnapShot = await getDocs(user)
      userSnapShot.forEach(doc => {
        this.user = doc.data().name
      })
    }
  },
  mounted() {
    this.getUser()
  }
}
</script>
