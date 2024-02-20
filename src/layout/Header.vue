<template>
  <div class="header">
    <h1>Where in the world?</h1>
    <ThemeToggle/>
  </div>
</template>

<script setup>
import ThemeToggle from "@/components/ThemeToggle.vue";
</script>

<style lang="scss" scoped>

@import '@/assets/styles/mixin';

.header {
  padding: 0 5%;
  background-color: var(--color-navbar);
  box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.1);

  position: fixed;
  width: 100%;
  height: var(--navbar-height);
  box-sizing: border-box;
  top: 0;
  left: 0;
  z-index: 100;

  display: flex;
  align-items: center;

  h1 {
    font-size: 24px;
    font-weight: var(--font-weight-bold);

    @include mobile {
      font-size: 18px;
    }
  }
}

:deep(.theme-toggle) {
  margin-left: auto;
}

</style>