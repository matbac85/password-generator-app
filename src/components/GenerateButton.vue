<template>
  <button @click.prevent="generatePassword">
    generate
    <svg width="12" height="12" xmlns="http://www.w3.org/2000/svg">
      <path
        fill="currentColor"
        d="m5.106 12 6-6-6-6-1.265 1.265 3.841 3.84H.001v1.79h7.681l-3.841 3.84z"
      />
    </svg>
  </button>
</template>

<script setup>
import { ref } from "vue";
import { defineProps, defineEmits } from "vue";

const emit = defineEmits(["passwordGenerated"]);

const props = defineProps({
  length: {
    type: Number,
    default: 12,
  },
  uppercase: {
    type: Boolean,
    default: true,
  },
  lowercase: {
    type: Boolean,
    default: true,
  },
  numbers: {
    type: Boolean,
    default: true,
  },
  symbols: {
    type: Boolean,
    default: true,
  },
});

const password = ref("");

const getRandomChar = (charSet) => {
  const randomIndex = Math.floor(Math.random() * charSet.length);
  return charSet[randomIndex];
};

const generatePassword = () => {
  const { length, uppercase, lowercase, symbols, numbers } = props;

  // Sets of characters to choose from
  const uppercaseChars = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
  const lowercaseChars = "abcdefghijklmnopqrstuvwxyz";
  const symbolChars = "!@#$%^&*()_+[]{}|;:,.<>?";
  const numberChars = "0123456789";

  // Collecting characters based on options
  let allChars = "";
  if (uppercase) allChars += uppercaseChars;
  if (lowercase) allChars += lowercaseChars;
  if (symbols) allChars += symbolChars;
  if (numbers) allChars += numberChars;

  // Generating the password
  let generatedPassword = "";
  for (let i = 0; i < length; i++) {
    generatedPassword += getRandomChar(allChars);
  }

  password.value = generatedPassword;
  emit("passwordGenerated", generatedPassword); // Emission de l'événement ici
};
</script>

<style scoped>
button {
  appearance: none;
  -webkit-appearance: none;
  background-color: var(--secondary);
  min-width: 100%;
  border: 2px solid transparent;
  color: var(--primary-500);
  margin-top: 1rem;
  display: flex;
  gap: 1rem;
  align-items: center;
  justify-content: center;
  padding: 1rem 0;
  text-transform: uppercase;
  font-size: clamp(1rem, 0.509vw + 0.881rem, 1.125rem);
  cursor: pointer;
}

button:hover {
  background-color: var(--primary-500);
  color: var(--secondary);
  border-color: var(--secondary);
}

@media (min-width: 37.5rem) {
  button {
    margin-top: 2rem;
  }
}
</style>
