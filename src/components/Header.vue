<script setup>
// No props or specific functionality for the header
</script>

<template>
  <header class="header">
    <div class="container">
      <h1 class="header-title">Activity Tracker</h1>
      <p class="header-subtitle">Organize your tasks and activities efficiently</p>
    </div>
  </header>
</template>

<style scoped>
.header {
  background-color: var(--primary-600);
  color: white;
  padding: var(--space-6) 0;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.header-title {
  color: white;
  margin-bottom: var(--space-2);
  font-weight: 600;
}

.header-subtitle {
  color: var(--primary-100);
  margin: 0;
  font-size: 1rem;
}
</style>