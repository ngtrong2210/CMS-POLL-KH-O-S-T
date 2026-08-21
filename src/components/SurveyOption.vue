<script setup>
defineProps({
  questionId: { type: Number, required: true },
  option: { type: Object, required: true },
  modelValue: { type: String, default: '' },
})

defineEmits(['update:modelValue'])
</script>

<template>
  <label
    class="survey-option"
    :class="{ 'survey-option--selected': modelValue === option.value }"
    :for="`question-${questionId}-${option.value}`"
  >
    <input
      :id="`question-${questionId}-${option.value}`"
      class="form-check-input"
      type="radio"
      :name="`question-${questionId}`"
      :value="option.value"
      :checked="modelValue === option.value"
      @change="$emit('update:modelValue', option.value)"
    />
    <span>{{ option.text }}</span>
  </label>
</template>
