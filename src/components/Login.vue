<template>
    <div class="login-form mt-5">
        <div v-show="error" class="alert alert-danger">
            {{ errorMsg }}
        </div>
        <div v-show="success" class="alert alert-success">
            {{ successMsg }}
        </div>
      <h2 class="login-form__title">Login</h2>
      <form class="login-form__form" @submit.prevent="loginUser">
        <div class="login-form__input-group">
          <label for="email" class="login-form__label">Email:</label>
          <input type="email" id="email" class="login-form__input" v-model="email">
        </div>
        <div class="login-form__input-group">
          <label for="password" class="login-form__label">Password:</label>
          <input type="password" id="password" class="login-form__input" v-model="password">
        </div>
        <div class="login-form__submit">
          <button class="login-form__button" type="submit">Login</button>
        </div>
      </form>
      <div class="login-form__register-link">
        <router-link to="register" style="color:black;">Don't have an account yet?</router-link>
      </div>
    </div>
  </template>

<script setup>
  import { ref } from 'vue'
  import { auth } from '../Firebase/index.js'
  import { signInWithEmailAndPassword } from '@firebase/auth'
  import router from '../router/index.js'

  const email = ref('')
  const password = ref('')
  const error = ref('')
  const errorMsg = ref('')
  const success = ref('')
  const successMsg = ref('')

  const loginUser = () => {
    if (email.value && password.value) {
      signInWithEmailAndPassword(auth, email.value, password.value)
        .then(() => {
          router.push('/home')
        })
        .catch((err) => {
          error.value = true
          errorMsg.value = err.message
        })
    } else {
      error.value = true
      errorMsg.value = 'Please fill out both email and password fields.'
    }
  }
</script>

<style scoped>
.login-form {
  max-width: 500px;
  margin: 0 auto;
  padding: 1.5rem;
  border: 2px solid #030303;
  border-radius: 0.5rem;
}

.login-form__title {
  text-align: center;
  margin-bottom: 1.5rem;
}

.login-form__form {
  display: flex;
  flex-direction: column;
}

.login-form__input-group {
  display: flex;
  flex-direction: column;
  margin-bottom: 1rem;
}

.login-form__label {
  font-weight: bold;
  margin-bottom: 0.5rem;
}

.login-form__input {
  padding: 0.5rem;
  border-radius: 0.5rem;
  border: 1px solid #ccc;
}

.login-form__submit {
  display: flex;
  justify-content: center;
  margin-top: 1rem;
}

.login-form__button {
  padding: 0.5rem 1rem;
  background-color: #007bff;
  color: #fff;
  border-radius: 0.5rem;
  border: none;
  cursor: pointer;
}

.login-form__register-link {
  text-align: center;
  margin-top: 1rem;
}
</style>