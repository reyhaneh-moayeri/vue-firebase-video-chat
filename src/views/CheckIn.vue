<template>
  <div id="check-in">
    <form class="mt-3" @submit.prevent="handleCheckIn">
      <div class="container mx-auto">
        <div v-if="user" class="">
          <h1>Check in</h1>
          <p class="mt-3 font-bold text-xl" v-if="roomName">
            To: <span class="">{{ roomName }}</span>
          </p>
          <section class="p-3">
            <label class="" for="displayName">Name</label>
            <input
              required
              class="shadow appearance-none border border-red rounded w-full py-2 px-3 text-grey-darker mb-3"
              type="text"
              v-model="displayName"
            />
          </section>
          <div class="text-right mb-0">
            <button class="bg-red-600 text-white font-bold p-2 rounder" type="submit">
              Check in
            </button>
          </div>
        </div>
        <div class="text-center" v-else>
          <h1 class="">Sorry</h1>
          <p class="">
            Sorry, access to rooms is only available to registered users. Please
            <router-link to="/login">login</router-link> or
            <router-link to="/register">register</router-link> and try again.
          </p>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
import db from '../db'
import { getAuth, onAuthStateChanged } from 'firebase/auth'
import { collection, doc, getDoc } from 'firebase/firestore'
export default {
  props: ['user'],
  data() {
    return {
      roomName: null,
      displayName: null
    }
  },
  methods: {
    handleCheckIn() {
      this.$emit('checkIn', {
        hostId: this.$route.params.hostId,
        roomId: this.$route.params.roomId,
        displayName: this.displayName
      })
    }
  },
  mounted() {
    const auth = getAuth()
    onAuthStateChanged(auth, user => {
      if (user) {
        this.displayName = user.displayName
        console.log(user)
      }
    })
    const userRef = doc(collection(db, 'users'), this.$route.params.hostId)
    const roomRef = doc(collection(userRef, 'rooms'), this.$route.params.roomId)
    getDoc(roomRef).then(doc => {
      if (doc.exists) {
        this.roomName = doc.data().name
        console.log('YES')
      } else {
        this.$router.replace('/')
        console.log('NO')
      }
    })
  }
}
</script>
