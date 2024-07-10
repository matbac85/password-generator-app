<template>
  <div>
    <Heading />
    <PasswordGenerator />
  </div>
</template>
<script setup>
import Heading from "./components/Heading.vue";
import PasswordGenerator from "./components/PasswordGenerator.vue";
</script>
<style scoped>
div {
  width: 100%;
  max-width: 33.75rem;
}
</style>
