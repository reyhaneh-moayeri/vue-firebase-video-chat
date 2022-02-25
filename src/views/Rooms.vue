<template>
  <div id="rooms" class="container mx-auto">
    <h1 class="text-red-500">Add a room</h1>
    <form class="relative mt-4">
      <input
        v-model="roomName"
        type="text"
        placeholder="room name"
        ref="roomName"
        class="shadow appearance-none border border-red rounded w-1/2 py-2 px-3 text-grey-darker mb-3"
      />
      <button
        type="submit"
        class="bg-blue-400 text-white py-2 px-3 relative right-9 rounded"
        id="buttonAdd"
        @click.prevent="handleAdd"
      >
        +
      </button>
    </form>

    <div class="w-1/2 bg-white rounded-lg shadow">
      <ul class="divide-y-2 divide-gray-100">
        <li
          class="my-3 py-1 px-2 flex justify-between align-center"
          v-for="room in rooms"
          :key="room.name"
        >
          <span>{{ room.name }}</span>
          <section class="btn-group align-self-center" role="group" aria-label="Room Options">
            <button
              class="p-1 mx-1 px-2 rounded border-gray-600 border-1 border-gray-200 text-gray-600 hover:bg-gray-600 hover:text-white"
              title="Delete Room"
              @click="$emit('deleteRoom', room.id)"
            >
              <font-awesome-icon icon="trash"></font-awesome-icon>
            </button>

            <router-link
              class="p-1 mx-1 px-2 rounded border-gray-600 border-1 border-gray-200 text-gray-600 hover:bg-gray-600 hover:text-white"
              title="Check In"
              :to="`/checkin/${user.uid}/${room.id}`"
            >
              <font-awesome-icon icon="user"></font-awesome-icon>
            </router-link>

            <router-link
              class="p-1 mx-1 px-2 rounded border-gray-200 border-1 border-gray-600 text-gray-600 hover:bg-gray-600 hover:text-white"
              title="Chat"
              to="/"
            >
              <font-awesome-icon icon="video"></font-awesome-icon>
            </router-link>
          </section>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import { FontAwesomeIcon } from '@fortawesome/vue-fontawesome'
export default {
  name: 'Rooms',
  props: ['rooms', 'user'],
  components: {
    FontAwesomeIcon
  },
  data() {
    return {
      roomName: null
    }
  },
  methods: {
    handleAdd() {
      this.$emit('addRoom', this.roomName)
      this.roomName = null
      this.$refs.roomName.focus()
    }
  }
}
</script>
