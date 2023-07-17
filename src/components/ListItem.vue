<script setup lang="ts">

export type Item = {
  id: number,
  value: string,
  completed: boolean,
}

  const props = defineProps({
    value: Object,
    onDelete: Function,
    onChange: Function,
  });

  function deletion() {
    if (props.onDelete !== undefined) {
      props.onDelete();
    }
  }

  function handleChange() {
    if (props.onChange !== undefined) {
      props.onChange();
    }
  }  
</script>

<template>
  <li>
    <span>{{ props.value?.value }}</span>
    <button @click="deletion">delete</button>
    <input type="checkbox" @change="handleChange" :checked="props.value?.completed" />
  </li>
</template>