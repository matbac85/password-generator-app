<template>
  <section>
    <output v-if="password" class="password-display">{{ password }}</output>
    <output v-else class="placeholder-display">{{ placeholder }}</output>

    <button @click="copyPassword">
      <span :class="{ 'fade-out': fadingOut }" v-if="copied">copied!</span>
      <svg v-else width="21" height="24" xmlns="http://www.w3.org/2000/svg">
        <path
          d="M20.341 3.091 17.909.659A2.25 2.25 0 0 0 16.319 0H8.25A2.25 2.25 0 0 0 6 2.25V4.5H2.25A2.25 2.25 0 0 0 0 6.75v15A2.25 2.25 0 0 0 2.25 24h10.5A2.25 2.25 0 0 0 15 21.75V19.5h3.75A2.25 2.25 0 0 0 21 17.25V4.682a2.25 2.25 0 0 0-.659-1.591ZM12.469 21.75H2.53a.281.281 0 0 1-.281-.281V7.03a.281.281 0 0 1 .281-.281H6v10.5a2.25 2.25 0 0 0 2.25 2.25h4.5v1.969a.282.282 0 0 1-.281.281Zm6-4.5H8.53a.281.281 0 0 1-.281-.281V2.53a.281.281 0 0 1 .281-.281H13.5v4.125c0 .621.504 1.125 1.125 1.125h4.125v9.469a.282.282 0 0 1-.281.281Zm.281-12h-3v-3h.451c.075 0 .147.03.2.082L18.667 4.6a.283.283 0 0 1 .082.199v.451Z"
          fill="currentColor"
        />
      </svg>
    </button>
  </section>
</template>

<script setup>
import { ref, watch } from "vue";
import { defineProps } from "vue";

const props = defineProps({
  password: {
    type: String,
    required: true,
  },
});

const placeholder = ref("PTx1f5DaFX");
const copied = ref(false);
const fadingOut = ref(false);

const copyPassword = async () => {
  try {
    await navigator.clipboard.writeText(props.password);
    copied.value = true;
    fadingOut.value = false;
    setTimeout(() => {
      fadingOut.value = true;
      setTimeout(() => {
        copied.value = false;
      }, 1000);
    }, 2000);
  } catch (err) {
    console.error("Failed to copy: ", err);
  }
};

watch(
  () => props.password,
  () => {
    copied.value = false;
    fadingOut.value = false;
  }
);
</script>

<style scoped>
section {
  background-color: var(--primary-500);
  padding: 1rem;
  min-width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: clamp(1rem, 2.036vw + 0.523rem, 1.5rem);
}

output {
  font-size: clamp(1.5rem, 2.036vw + 1.023rem, 2rem);
}

.placeholder-display {
  color: var(--primary-300);
  opacity: 0.25;
}

.password-display {
  color: var(--primary-100);
}

button {
  all: unset;
  cursor: pointer;
  color: var(--secondary);
  width: fit-content;
  display: flex;
  align-items: center;
  justify-content: center;
}

button:hover {
  color: var(--primary-100);
}

span {
  text-transform: uppercase;
  transition: opacity 1s ease-in-out;
  opacity: 1;
}

span.fade-out {
  opacity: 0;
}

@media (min-width: 37.5rem) {
  section {
    padding: 1.1875rem 2rem;
  }
}
</style>
