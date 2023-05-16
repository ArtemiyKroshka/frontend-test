<script setup>
import { ref, defineEmits } from "vue";

const emit = defineEmits(["input"]);

const inputValue = ref("");

const updateInputValue = (event) => {
  inputValue.value = event.target.value;
  emit("input", inputValue.value);
};
</script>

<template>
  <div>
    <input
      data-test="password-field"
      v-model="inputValue"
      @input="updateInputValue"
    />
  </div>
</template>