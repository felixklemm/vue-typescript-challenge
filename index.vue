<script setup lang="ts">
import { computed, ref } from 'vue'

type User = {
  id: number
  name: string
  age?: number
  role: 'admin' | 'user'
  active: boolean
}

const users = ref<User[]>([
  { id: 1, name: 'Anna', age: 28, role: 'admin', active: true },
  { id: 2, name: 'Max', role: 'user', active: true }
])

const search = ref<string>('')

const selectedUser = ref<User>()

function getUser(id: string): User {
  return users.value.find(user => user.id === id)
}

function updateUser(user: User, property: string, value: any) {
  user[property] = value
}

function getAge(user: User) {
  return user.age.toFixed(0)
}

const filteredUsers = computed(() => {
  return users.value.filter(user =>
    user.name.toLowerCase().includes(search.value)
  )
})

function selectUser(id: number | string) {
  selectedUser.value = users.value.find(user => user.id == id)

  console.log(selectedUser.value.name)
}

function deactivateUser(userId: number) {
  const user = users.value.find(user => user.id === userId)

  user.active = false
}

const activeUsers = computed<User[]>(() => {
  return users.value.filter(user => user.active).length
})
</script>

<template>
  <input v-model="search" />

  <div
    v-for="(user, index) in filteredUsers"
    :key="index"
  >
    {{ user.name }} – {{ getAge(user) }}

    <button @click="selectUser(user.id)">
      Select
    </button>

    <button @click="deactivateUser(user.id)">
      Deactivate
    </button>
  </div>

  <p v-if="selectedUser">
    Selected: {{ selectedUser.name }}
  </p>

  <p>
    Active users: {{ activeUsers }}
  </p>
</template>