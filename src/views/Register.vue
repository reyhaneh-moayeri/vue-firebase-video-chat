<template>
  <div>
    <form class="mt-3" @submit.prevent="register">
      <div class="bg-white shadow-md rounded px-8 pt-6 pb-8 mb-4 flex flex-col max-w-xl mx-auto">
        <div class="alert alert-danger px-3" v-if="error">
          {{ error }}
        </div>
        <div class="mb-4">
          <label class="block text-grey-darker text-sm font-bold mb-2" for="email"> Name </label>
          <input
            class="shadow appearance-none border rounded w-full py-2 px-3 text-grey-darker"
            id="email"
            type="text"
            placeholder="Email"
            v-model="name"
          />
        </div>

        <div class="mb-4">
          <label class="block text-grey-darker text-sm font-bold mb-2" for="email"> Email </label>
          <input
            class="shadow appearance-none border rounded w-full py-2 px-3 text-grey-darker"
            id="email"
            type="text"
            placeholder="Email"
            v-model="email"
          />
        </div>
        <div class="mb-6">
          <label class="block text-grey-darker text-sm font-bold mb-2" for="password">
            Password
          </label>
          <input
            class="shadow appearance-none border border-red rounded w-1/2 py-2 px-3 text-grey-darker mb-3"
            id="password"
            type="password"
            placeholder="******************"
            v-model="passOne"
          />
          <input
            class="shadow appearance-none border border-red rounded w-1/2 py-2 px-3 text-grey-darker mb-3"
            id="password"
            type="password"
            placeholder="******************"
            v-model="passTow"
          />
          <p class="text-red text-xs italic">Please choose a password.</p>
        </div>
        <div class="flex items-center justify-between">
          <button class="text-white bg-blue-500 font-bold py-2 px-4 rounded" type="submit">
            Sign In
          </button>
        </div>
        <p class="text-center mt-2">
          or
          <router-link to="/login">Log In</router-link>
        </p>
      </div>
    </form>
  </div>
</template>

<script>
import { getAuth, createUserWithEmailAndPassword, updateProfile } from 'firebase/auth'
export default {
  data() {
    return {
      name: null,
      email: null,
      passOne: null,
      passTow: null,
      error: null
    }
  },
  methods: {
    register() {
      const info = {
        email: this.email,
        password: this.passTow,
        name: this.name
      }
      if (!this.error) {
        const auth = getAuth()
        createUserWithEmailAndPassword(auth, info.email, info.password).then(userCredential => {
          updateProfile(auth.currentUser, {
            displayName: info.name
          })
            .then(() => {
              this.$router.replace('/')
            })
            .catch(error => {
              this.error = error.message
            })
        })
      }
    }
  },
  watch: {
    passTow: function () {
      if (this.passOne !== '' && this.passTow !== '' && this.passOne !== this.passTow) {
        this.error = 'Passwords do not match'
      } else {
        this.error = null
      }
    }
  }
}
</script>
