<template>
  <div class="container">
    <div class="p-20">
      <h3 class="mb-10">Formulaire</h3>
      <form @submit="mySubmit">
        <input v-model="nameValue" class="mr-10" type="text" placeholder="Prénom" />
        <input v-model="emailValue" class="mr-10" type="text" placeholder="Email" />
        <button class="btn btn-primary">Sauvegarder</button>
      </form>
    </div>
    <div class="p-20">
      <h3>Liste des utilisateurs</h3>
      <ul>
        <li v-for="user in state.users">
          <p>{{ user.name }} - {{ user.email }}</p>
        </li>
      </ul>
    </div>
  </div>
</template>

<script setup lang="ts">
import { useForm, useField } from 'vee-validate'
import { reactive } from 'vue'

interface User {
  name: string
  email: string
  createdAt?: string
  _id?: string
}

const state = reactive<{ users: User[] }>({
  users: [],
})

const { handleSubmit, resetForm } = useForm()

const mySubmit = handleSubmit(async (value) => {
  try {
    const response = await fetch('https://restapi.fr/api/vueuser', {
      method: 'POST',
      body: JSON.stringify(value),
      headers: {
        'Content-Type': 'application/json',
      },
    })
    const user: User = await response.json()
    state.users.push(user)
    resetForm()
  } catch (err) {
    console.error(err)
  }
})

const { value: emailValue } = useField('email')
const { value: nameValue } = useField('name')
</script>

<style lang="scss">
@import './assets/scss/base.scss';
</style>
