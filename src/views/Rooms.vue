<template>
  <div id="rooms" class="container">
    <h1 class="text-red-500">Add a room</h1>
    <form class="relative mt-4">
      <input
        v-model="roomName"
        type="text"
        placeholder="room name"
        ref="roomName"
        class="shadow absolute appearance-none border border-red rounded w-full py-2 px-3 text-grey-darker mb-3"
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
  </div>
</template>

<script>
export default {
  name: 'Rooms',
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
