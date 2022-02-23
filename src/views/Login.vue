<template>
  <div>
    <form class="mt-3" @submit.prevent="login">
      <div class="bg-white shadow-md rounded px-8 pt-6 pb-8 mb-4 flex flex-col max-w-xl mx-auto">
        <div class="alert alert-danger px-3" v-if="error">
          {{ error }}
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
            class="shadow appearance-none border border-red rounded w-full py-2 px-3 text-grey-darker mb-3"
            id="password"
            type="password"
            placeholder="******************"
            v-model="password"
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
          <router-link to="/register">register</router-link>
        </p>
      </div>
    </form>
  </div>
</template>

<script>
import { getAuth, signInWithEmailAndPassword } from 'firebase/auth'
export default {
  name: 'Login',
  data() {
    return {
      email: null,
      password: null,
      error: null
    }
  },
  methods: {
    login() {
      const info = {
        email: this.email,
        password: this.password
      }
      const auth = getAuth()
      // example@gmail.com
      // 123456
      signInWithEmailAndPassword(auth, info.email, info.password).then(
        userCredential => {
          const user = userCredential.user
          this.$router.push('/rooms')
        },
        error => {
          this.error = error.message
        }
      )
    }
  }
}
</script>
