<script setup>
import { RULE } from "@/domain/password/rules";
import { computed, defineProps, ref } from "vue";

const props = defineProps(["password", "rules"]);

const rulesText = ref([
  { value: "OneLetter", text: "Has at least one letter" },
  {
    value: "UpperAndLower",
    text: "Has at least one lower and one upper case letter",
  },
  { value: "OneNumber", text: "Has at least one number" },
  { value: "SpecialSymbol", text: "Has at least one special character" },
  { value: "LongerThan4", text: "Has length > 4" },
  { value: "LongerThan8", text: "Has length > 8" },
  { value: "LongerThan12", text: "Has length > 12" },
]);

const checkPass = (rule) => {
  switch (rule) {
    case RULE.OneLetter:
      return /[a-zA-Z]/.test(props.password);
    case RULE.UpperAndLower:
      return /^(?=.*[a-z])(?=.*[A-Z])/.test(props.password);
    case RULE.OneNumber:
      return /[0-9]/.test(props.password);
    case RULE.SpecialSymbol:
      return /[!@#$%^&*(),.?":{}|<>_-]|a[\s+]p/.test(props.password);
    case RULE.LongerThan4:
      return props.password.length > 4;
    case RULE.LongerThan8:
      return props.password.length > 8;
    case RULE.LongerThan12:
      return props.password.length > 12;
    default:
      return false;
  }
};

const validationStatus = computed(() => {
  const fulfilledRules = props.rules.filter((rule) => checkPass(rule));
  return fulfilledRules.length >= 5 ? "strong enough" : "weak";
});

const selectedRuleText = (selectedRule) => {
  return rulesText.value.find((rule) => rule.value === selectedRule).text;
};
</script>

<template>
  <div>
    <ul>
      <li
        v-for="rule in props.rules"
        :key="rule"
        :data-test-rule-indicator="rule"
        class="password-hint__rule"
        :class="{
          'password-hint__rule--pass': checkPass(rule),
          'password-hint__rule--fail': !checkPass(rule),
        }"
      >
        {{ selectedRuleText(rule) }}
      </li>
    </ul>

    <span data-test="validation-summary"
      >Password is
      <span
        class="password-check"
        :class="{ password__strong: validationStatus === 'strong enough' }"
        >{{ validationStatus }}</span
      ></span
    >
  </div>
</template>