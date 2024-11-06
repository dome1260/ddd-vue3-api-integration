<template>
  <div>
    <button @click="setModal(true)">
      Delete
    </button>
    <div
      v-if="modal"
      class="modal"
      @click="setModal(false)">
      <div class="modal-content" @click.stop>
        <button @click="handelDeleteUser(false)">
          Confirm
        </button>
        <button @click="setModal(false)">
          Cancel
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import axios from 'axios'
import { ref } from 'vue'

const urlPrefix = 'https://67236aa3493fac3cf24acd03.mockapi.io'

const modal = ref(false)

const props = defineProps({
  user: {
    type: Object,
    default: () => ({})
  }
})

const emit = defineEmits(['delete'])

const setModal = (value) => {
  modal.value = value
}

const handelDeleteUser = async () => {
  try {
    await axios.delete(`${urlPrefix}/users/${props.user.id}`)
    emit('delete')
    setModal(false)
  } catch (error) {
    console.error('[ERROR] handle delete user', error)
  }
}
</script>

<style scoped>
.modal {
  position: fixed;
  z-index: 1005;
  left: 0;
  top: 0;
  /* viewport width */
  width: 100vw;
   /* viewport height */
  height: 100vh;
  background-color: rgba(0,0,0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-content {
  min-width: 300px;
  max-width: 80%;
  padding: 16px;
  box-shadow: 0 4px 8px rgb(0, 0, 0, 0.2);
  background-color: white;
  border-radius: 8px;
  display: flex;
  justify-content: center;
  gap: 16px
}

button {
  padding: 4px 16px;
}

label {
  display: block;
  margin-bottom: 8px;
}

input {
  width: 100%;
  padding: 4px;
  margin-bottom: 16px;
  border: 1px solid #ccc;
  border-radius: 4px;
  padding: 4px 8px;
}
</style>
