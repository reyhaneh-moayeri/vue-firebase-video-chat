<template>
  <div id="app">
    <Nav @signout="signOut" :user="user" />
    <router-view :user="user" @signout="signOut" @addRoom="addRoom" />
  </div>
</template>

<script>
import Nav from './components/Nav.vue'
import db from './db.js'
import Firebase from 'firebase/firestore'
import { collection, doc, addDoc } from 'firebase/firestore'
import { getAuth, onAuthStateChanged, signOut } from 'firebase/auth'
export default {
  name: 'Home',
  components: {
    Nav
  },
  data() {
    return {
      user: null
    }
  },

  methods: {
    async getUser() {
      const auth = getAuth()
      onAuthStateChanged(auth, user => {
        if (user) {
          this.user = user
        } else {
          // User is signed out
        }
      })
    },

    signOut() {
      const auth = getAuth()
      signOut(auth)
        .then(() => {
          this.user = null
          console.log('user signed out successfully')
          this.$router.push('/login')
        })
        .catch(error => {
          console.log('error signing out', error)
        })
    },
    async addRoom(payload) {
      const t = doc(collection(db, 'users'), this.user.uid)
      await addDoc(collection(t, 'rooms'), {
        name: payload,
        createdAt: new Date().toDateString()
      })
    }
  },
  mounted() {
    this.getUser()
  }
}
</script>

<style lang="scss">
$primar: '#5f2882';
@import './node_modules/bootstrap/scss/bootstrap';
</style>
