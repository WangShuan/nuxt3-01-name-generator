<template>
  <li>{{ name }}<span @click="emit('removeName')">❌</span></li>
</template>

<script setup lang="ts">
interface NameProps {
  name: string;
}
const props = defineProps<NameProps>()

const emit = defineEmits<{
  (e: 'removeName'): void
}>()

</script>

<style scoped>
li {
  background: #434343;
  color: #fff;
  margin: 5px;
  padding: 0.35rem 0.75rem;
  border-radius: 10px;
  position: relative;
}

li>span {
  display: inline-block;
  margin-left: 0.35rem;
  cursor: pointer;
}
</style>