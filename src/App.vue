<template>
  <div id="app">
    <Nav @signout="signOut" :user="user" />
    <router-view
      :user="user"
      @signout="signOut"
      :rooms="rooms"
      @addRoom="addRoom"
      @deleteRoom="deleteRoom"
    />
  </div>
</template>

<script>
import Nav from './components/Nav.vue'
import db from './db.js'
import { collection, doc, addDoc, getDocs, deleteDoc } from 'firebase/firestore'
import { getAuth, onAuthStateChanged, signOut } from 'firebase/auth'
export default {
  name: 'Home',
  components: {
    Nav
  },
  data() {
    return {
      user: null,
      rooms: []
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
      const userDoc = doc(collection(db, 'users'), this.user.uid)
      await addDoc(collection(userDoc, 'rooms'), {
        name: payload,
        createdAt: new Date().toDateString()
      })
    },

    getRoomsData() {
      const auth = getAuth()
      auth.onAuthStateChanged(user => {
        if (user) {
          const userRef = doc(collection(db, 'users'), this.user.uid)
          const querySnapshot = getDocs(collection(userRef, 'rooms'))
          querySnapshot.then(res => {
            const snapData = []
            res.forEach(doc => {
              snapData.push({
                id: doc.id,
                name: doc.data().name,
                createdAt: doc.data().createdAt
              })
            })

            this.rooms = snapData.sort((a, b) => {
              if (a.name.toLowerCase() < b.name.toLowerCase()) {
                return -1
              } else {
                return 1
              }
            })
          })
        }
      })
    },
    async deleteRoom(payload) {
      const userDoc = doc(collection(db, 'users'), this.user.uid)
      await deleteDoc(doc(collection(userDoc, 'rooms'), payload))
    }
  },
  watch: {
    rooms: function () {
      this.getRoomsData()
    }
  },
  mounted() {
    this.getUser()
    this.getRoomsData()
  }
}
</script>
