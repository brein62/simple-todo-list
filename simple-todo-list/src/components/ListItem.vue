<script setup lang="ts">

export type Item = {
  id: number,
  value: string,
}

  const props = defineProps({
    value: Object,
    onDelete: Function
  });

  function deletion() {
    if (props.onDelete !== undefined) {
      props.onDelete();
    }
  }
</script>

<template>
  <li>{{ props.value }} <button @click="deletion">delete</button></li>
</template>