<template>
  <slot />

  <hr />

  <!-- <footer>
    <br />
    Using Vue v{{ vueVersion }} and Pinia v{{ piniaVersion }}.
  </footer> -->
</template>

<script setup lang="ts">
import { version as piniaVersion } from 'pinia/package.json';
import { version as vueVersion } from 'vue/package.json';
</script>

<style scoped></style>
