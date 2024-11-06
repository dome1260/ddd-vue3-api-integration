<template>
  <div>
    <button @click="setModal(true)">
      Edit
    </button>
    <div
      v-if="modal"
      class="modal"
      @click="setModal(false)">
      <div class="modal-content" @click.stop>
        <h1> Update </h1>
        <div>
          <label> First Name </label>
          <input v-model="form.firstName" type="text">
        </div>
        <div>
          <label> Last Name </label>
          <input v-model="form.lastName" type="text">
        </div>
        <div>
          <label> Phone Number </label>
          <input v-model="form.phoneNumber" type="text">
        </div>
        <div>
          <button @click="handelUpdateUser()">
            Save
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import axios from 'axios';
import { ref, reactive, watch } from 'vue'

const urlPrefix = 'https://67236aa3493fac3cf24acd03.mockapi.io'

const modal = ref(false)
const form = reactive({
  firstName: '',
  lastName: '',
  phoneNumber: ''
})

const props = defineProps({
  user: {
    type: Object,
    default: () => ({})
  }
})

const emit = defineEmits(['update'])

watch(
  modal,
  (_newVal) => {
    if (_newVal) {
      form.firstName = props.user.firstName
      form.lastName = props.user.lastName
      form.phoneNumber = props.user.phoneNumber
    }
  }
)

const setModal = (value) => {
  modal.value = value
}

// const clearForm = () => {
//   form.firstName = ''
//   form.lastName = ''
//   form.phoneNumber = ''
// }

const handelUpdateUser = async () => {
  try {
    await axios.put(`${urlPrefix}/users/${props.user.id}`, form)
    emit('update')
    setModal(false)
    // clearForm()
  } catch (error) {
    console.error('[ERROR] handle update user', error)
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
