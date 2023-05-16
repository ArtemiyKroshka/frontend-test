<script setup>
import { RULE } from "@/domain/password/rules";
import { ref } from "vue";
import PasswordInput from "./components/passwordInput.vue";
import PasswordHints from "./components/passwordHints.vue";

const password = ref("");

const rules = Object.values(RULE);

const handlePasswordChange = (value) => {
  password.value = value;
};
</script>

<template>
  <main class="container">
    <div class="password-block">
      <PasswordInput @input="handlePasswordChange" />

      <PasswordHints :password="password" :rules="rules" />
    </div>
  </main>
</template>