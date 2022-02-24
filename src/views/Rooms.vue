<template>
  <div id="rooms" class="container">
    <h1 class="text-red-500">Add a room</h1>
    <form class="relative mt-4">
      <input
        v-model="roomName"
        type="text"
        placeholder="room name"
        ref="roomName"
        class="shadow appearance-none border border-red rounded w-full py-2 px-3 text-grey-darker mb-3"
      />
      <button
        type="submit"
        class="bg-blue-400 text-white absolute py-2 px-3 right-0 rounded"
        id="buttonAdd"
        @click.prevent="handleAdd"
      >
        +
      </button>
    </form>

    <div class="w-full bg-white rounded-lg shadow">
      <ul class="divide-y-2 divide-gray-100">
        <li
          class="my-3 py-1 px-2 flex justify-between align-center"
          v-for="room in rooms"
          :key="room.name"
        >
          <span>created By: {{ room.name }}</span>
          <span>{{ room.id }}</span>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Rooms',
  props: ['rooms'],
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
