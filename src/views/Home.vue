<template>
  <div class="home">
    <h3>this is home page</h3>
    <p>welcome {{ user }}</p>
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
