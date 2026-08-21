<script setup>
import { computed } from 'vue'

const props = defineProps({
  fields: { type: Array, required: true },
  modelValue: { type: Object, required: true },
})

const emit = defineEmits(['update:modelValue'])

const groups = computed(() => {
  const grouped = new Map()

  props.fields.forEach((field) => {
    if (!grouped.has(field.group)) grouped.set(field.group, [])
    grouped.get(field.group).push(field)
  })

  return Array.from(grouped, ([title, fields]) => ({ title, fields }))
})

function updateField(id, value) {
  emit('update:modelValue', {
    ...props.modelValue,
    [id]: value,
  })
}
</script>

<template>
  <section class="student-info" :aria-label="groups.map((group) => group.title).join(' - ')">
    <div v-for="group in groups" :key="group.title" class="student-info__group">
      <h2 class="student-info__title">{{ group.title }}</h2>
      <div class="student-info__fields">
        <div v-for="field in group.fields" :key="field.id" class="form-field">
          <label class="form-label" :for="field.id">{{ field.label }}</label>
          <input
            :id="field.id"
            :type="field.type"
            class="form-control"
            :value="modelValue[field.id]"
            autocomplete="off"
            @input="updateField(field.id, $event.target.value)"
          />
        </div>
      </div>
    </div>
  </section>
</template>
