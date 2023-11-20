<template>
  <div class="users">
    <div class="container">
      <section>
        <h5 class="title">Novo usuário</h5>
        <form @submit.prevent="submitNewUser">
          <input v-model="form.name" type="text" placeholder="Nome">
          <input v-model="form.email" type="text" placeholder="E-mail">
          <button type="submit">Adicionar</button>
        </form>
      </section>
      <section>
        <h5 class="title">Lista de usuários</h5>
        <ul>
          <li v-for="(user, i) in users" :key="i">
            <p>{{ user.name }}</p>
            <small>{{ user.email }}</small>
            <a class="destroy"></a>
          </li>
        </ul>
      </section>
    </div>
  </div>
</template>

<script setup lang="ts">
import axios from './utils/axios'
import { onMounted, ref, reactive } from 'vue';

interface User {
  id: string,
  name: string,
  email: string
}

onMounted(() => {
  fetchUsers()
})

const form = reactive({
  name: '',
  email: ''
})

const users = ref<User[]>([])

async function fetchUsers() {
  try {
    const { data } = await axios.get('/users')
    users.value = data
  } catch (error) {
    console.warn(error)
  }
}

async function createUser() {
  try {
    await axios.post('/users', form)
    form.name = ''
    form.email = ''
  } catch (error) {
    console.warn(error)
  } 
}

async function submitNewUser() {
  await createUser()
  await fetchUsers()
}



</script>

<style scoped>
.container {
  margin: 4rem auto;
  max-width: 500px;
  width: 90%;
  display: grid;
  grid-gap: 2.5rem;
}

.title {
  font-size: 1rem;
  font-weight: 500;
  margin: 0.7rem 0;
}

form {
  display: grid;
  grid-gap: 1rem;
}

input {
  background-color: transparent;
  border: 1px solid #999fc6;
  border-radius: 1rem;
  padding: 0.6rem;
  outline: none;
  color: #e1e8ef;
}


input::placeholder {
  color: #999fc6;
}

button {
  background-color: #2d6cea;
  color: #e1e8ef;
  border: none;
  border-radius: 1rem;
  padding: 0.6rem 1.5rem;
  width: max-content;
  transition: all 0.3s linear;
  outline: none;
  cursor: pointer;
  box-shadow: 0 0 5px 3px rgba(45, 108, 234, 0.3);
}

button:hover {
  background-color: #1b5cdc;
}

p {
  margin: 0;
}

ul {
  padding: 0;
  margin: 0;
  display: grid;
  grid-gap: 1rem;
}

li {
  background-color: #2b3a4e;
  padding: 1.2rem 1rem;
  border-radius: 1rem;
  position: relative;
  list-style: none;
  color: #8b98a8
}


.destroy {
  background-color: #d53e6b;
  width: 24px;
  height: 24px;
  border-radius: 0.5rem;
  cursor: pointer;
  transition: all 0.2s linear;
  position: absolute;
  top: 35%;
  transform: translate(-50%);
  right: 1.3rem;
}

.destroy:before,
.destroy:after {
  content: "";
  width: 3px;
  height: 13px;
  background-color: #ececf6;
  position: absolute;
  top: 50%;
  left: 50%;
}

.destroy:before {
  transform: translate(-50%, -50%) rotate(45deg);
}

.destroy:after {
  transform: translate(-50%, -50%) rotate(130deg);
}

.destroy:hover {
  background-color: #984848;
}
</style>