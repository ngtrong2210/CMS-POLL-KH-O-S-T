<script setup>
import SurveyOption from './SurveyOption.vue'

defineProps({
  question: { type: Object, required: true },
  modelValue: { type: String, default: '' },
})

defineEmits(['update:modelValue'])
</script>

<template>
  <fieldset class="survey-question">
    <legend class="survey-question__legend">
      <span class="survey-question__number">Câu {{ question.id }}</span>
      <span class="survey-question__text">{{ question.text }}</span>
    </legend>
    <div class="survey-question__options">
      <SurveyOption
        v-for="option in question.options"
        :key="option.value"
        :question-id="question.id"
        :option="option"
        :model-value="modelValue"
        @update:model-value="$emit('update:modelValue', $event)"
      />
    </div>
  </fieldset>
</template>
