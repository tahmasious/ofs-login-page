<template>
  <form @submit.prevent="submit">
    <v-text-field
      v-model="email.value.value"
      :error-messages="email.errorMessage.value"
      label="E-mail"
    ></v-text-field>

    <v-text-field
      v-model="password.value.value"
      :error-messages="password.errorMessage.value"
      label="password"
      type="password"
    ></v-text-field>
    <p>{{ serverError }}</p>
    <v-btn class="me-4" type="submit"> submit </v-btn>
  </form>
</template>
<script setup lang="ts">
import { useField, useForm } from "vee-validate";
import axios from "axios";
import { ref } from "Vue";
const { handleSubmit, handleReset } = useForm({
  validationSchema: {
    email(value) {
      if (/^[a-z.-]+@[a-z.-]+\.[a-z]+$/i.test(value)) return true;

      return "Must be a valid e-mail.";
    },
  },
});

const email = useField("email");
const password = useField("password");
const serverError = ref("");
interface loginData {
  email: string;
  password: string;
}

const submit = handleSubmit((vals: loginData) => {
  axios
    .post("https://dummyjson.com/products/add", vals)
    .then((res) => {
      alert("success");
    })
    .catch((e) => {
      serverError.value = e.message;
    });
});
</script>
