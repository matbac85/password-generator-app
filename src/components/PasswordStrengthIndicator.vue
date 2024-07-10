<template>
  <div class="password-strength-indicator">
    <p>strength</p>
    <div class="output">
      <output>{{ strengthText }}</output>
      <div class="strength-bars" :class="strengthClass">
        <div
          v-for="(bar, index) in 4"
          :key="index"
          :class="{ filled: index < strengthLevel }"
        ></div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { computed } from "vue";

const props = defineProps({
  length: Number,
  uppercase: Boolean,
  lowercase: Boolean,
  numbers: Boolean,
  symbols: Boolean,
});

const calculatePasswordEntropy = (
  length,
  uppercase,
  lowercase,
  numbers,
  symbols
) => {
  let P = 0;
  if (uppercase) P += 26;
  if (lowercase) P += 26;
  if (numbers) P += 10;
  if (symbols) P += 33;

  if (P === 0 || length === 0) return 0;

  return length * Math.log2(P);
};

const getPasswordStrength = (entropy) => {
  if (entropy < 28) return "too weak";
  if (entropy < 36) return "weak";
  if (entropy < 60) return "medium";
  return "strong";
};

const entropy = computed(() =>
  calculatePasswordEntropy(
    props.length,
    props.uppercase,
    props.lowercase,
    props.numbers,
    props.symbols
  )
);

const strengthText = computed(() => getPasswordStrength(entropy.value));

const strengthLevel = computed(() => {
  switch (strengthText.value) {
    case "too weak":
      return 1;
    case "weak":
      return 2;
    case "medium":
      return 3;
    case "strong":
      return 4;
    default:
      return 0;
  }
});

const strengthClass = computed(() => {
  return strengthText.value.replace(" ", "-");
});
</script>

<style scoped>
.password-strength-indicator {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background-color: var(--primary-900);
  padding: 0.875rem 1rem;
}

p {
  font-size: clamp(1rem, 0.509vw + 0.881rem, 1.125rem);
  text-transform: uppercase;
  color: var(--primary-300);
}

output {
  font-size: clamp(1.125rem, 1.527vw + 0.767rem, 1.5rem);
  text-transform: uppercase;
  color: var(--primary-100);
}

.output {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.strength-bars {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.strength-bars div {
  width: 0.625rem;
  height: 1.75rem;
  background-color: var(--primary-100);
}

.strength-bars div.filled {
  background-color: var(--secondary);
}

.strength-bars.too-weak div.filled {
  background-color: var(--too-weak);
}

.strength-bars.weak div.filled {
  background-color: var(--weak);
}

.strength-bars.medium div.filled {
  background-color: var(--medium);
}

.strength-bars.strong div.filled {
  background-color: var(--strong);
}
</style>
