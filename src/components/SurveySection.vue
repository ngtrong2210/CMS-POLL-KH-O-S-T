<script setup>
import SurveyQuestion from './SurveyQuestion.vue'

defineProps({
  section: { type: Object, required: true },
  answers: { type: Object, required: true },
})

defineEmits(['answer'])
</script>

<template>
  <section class="survey-section" :aria-labelledby="`${section.id}-title`">
    <header class="survey-section__header">
      <span class="survey-section__label">{{ section.label }}</span>
      <h3 :id="`${section.id}-title`">{{ section.title }}</h3>
    </header>

    <div class="survey-section__questions">
      <SurveyQuestion
        v-for="question in section.questions"
        :key="question.id"
        :question="question"
        :model-value="answers[question.id]"
        @update:model-value="$emit('answer', question.id, $event)"
      />
    </div>
  </section>
</template>
