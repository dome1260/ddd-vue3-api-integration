<template>
  <div class="home">
    <div class="home-header">
      <h1> User </h1>
      <CreateModal @create="getUsers()" />
    </div>
    <table>
      <thead>
        <tr>
          <th> First Name </th>
          <th> Last Name </th>
          <th> Phone Number </th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="(item, i) in items"
          :key="`user-${i + 1}`">
          <td> {{ item.firstName }} </td>
          <td> {{ item.lastName }} </td>
          <td> {{ item.phoneNumber }} </td>
          <td>
            <div>
              <UpdateModal
                :user="item"
                @update="getUsers()" />
              <DeleteModal
                :user="item"
                @delete="getUsers()" />
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import axios from 'axios'
import { ref, onMounted } from 'vue'

import CreateModal from '@/components/home/CreateModal.vue'
import UpdateModal from '@/components/home/UpdateModal.vue'
import DeleteModal from '@/components/home/DeleteModal.vue'

const urlPrefix = 'https://67236aa3493fac3cf24acd03.mockapi.io'

const items = ref([])

const getUsers = async () => {
  try {
    const response = await axios.get(`${urlPrefix}/users?sortBy=id&order=desc`)
    items.value = response.data
  } catch (error) {
    console.error('[ERROR] get users =>', error)
  }
}

onMounted(() => {
  getUsers()
})

</script>

<style scoped>
.home {
  display: flex;
  flex-direction: column;
  gap: 16px;
  padding: 8px 16px;
}

.home-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 16px;
}

table {
  width: 100%;
  border-collapse: collapse;
}

table thead tr th,
table tbody tr td {
  padding: 4px 16px;
  border: 1px solid black;
}

table tbody tr td:last-child div {
  display: flex;
  gap: 16px;
}

button {
  padding: 4px 16px;
}
</style>
