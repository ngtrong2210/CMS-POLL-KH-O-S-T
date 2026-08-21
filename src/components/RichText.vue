<script setup>
defineProps({
  runs: {
    type: Array,
    required: true,
  },
})
</script>

<template>
  <template v-for="(run, index) in runs" :key="index">
    <strong v-if="run.bold && !run.italic">{{ run.text }}</strong>
    <em v-else-if="run.italic && !run.bold">{{ run.text }}</em>
    <strong v-else-if="run.bold && run.italic"><em>{{ run.text }}</em></strong>
    <span v-else>{{ run.text }}</span>
  </template>
</template>
