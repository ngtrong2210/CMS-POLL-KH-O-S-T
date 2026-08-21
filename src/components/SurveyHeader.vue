<script setup>
defineProps({
  documentLabel: { type: String, required: true },
  title: { type: String, required: true },
  subtitle: { type: String, required: true },
})
</script>

<template>
  <header class="survey-header">
    <span class="survey-header__ornament" aria-hidden="true">
      <i class="bi bi-flower1"></i>
    </span>
    <p class="survey-header__label">{{ documentLabel }}</p>
    <h1>{{ title }}</h1>
    <p class="survey-header__subtitle">{{ subtitle }}</p>
  </header>
</template>
