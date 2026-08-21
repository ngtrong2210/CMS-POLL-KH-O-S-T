<script setup>
import RichText from './RichText.vue'

defineProps({
  title: { type: String, required: true },
  introduction: { type: Array, required: true },
  instructions: { type: Array, required: true },
})
</script>

<template>
  <section class="survey-intro" aria-labelledby="instructions-title">
    <h2 id="instructions-title" class="document-heading">{{ title }}</h2>
    <p class="survey-intro__lead"><RichText :runs="introduction" /></p>
    <ul class="survey-intro__list">
      <li v-for="(instruction, index) in instructions" :key="index">
        <RichText :runs="instruction" />
      </li>
    </ul>
  </section>
</template>
