<script setup>
const props = defineProps({
  msg: String
})

const emit = defineEmits(['response'])

// The first argument to emit() is the event name. Any additional arguments are passed on to the event listener.
emit('response', 'hello from child')

</script>

<template>
  <h2>Child component</h2>
    <h3>{{ msg || 'No props passed yet' }}</h3>
    <slot>Fallback content</slot>
  </template>