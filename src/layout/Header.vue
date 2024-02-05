<template>
  <div class="header">
    <h1>Where in the world?</h1>
    <ThemeToggle/>
  </div>
</template>

<script setup>
import ThemeToggle from "@/components/ThemeToggle.vue";
</script>

<style scoped>

.header {
  display: flex;
  align-items: center;

  height: 80px;

  background-color: var(--color-navbar);
  box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.1);

  padding: 0 5%;
}

:deep(.theme-toggle) {
  margin-left: auto;
}

</style>