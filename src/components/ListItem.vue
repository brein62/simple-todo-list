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
  <tr>
    <td>
      {{ props.value?.id.toString() }}
    </td>
    <td v-if="!props.value?.completed">
      {{ props.value?.value }}
    </td>
    <td v-else>
      <s>{{ props.value?.value }}</s>
    </td>
    <td>
      <button @click="deletion">delete</button>
    </td>
    <td>
      <input type="checkbox" @change="handleChange" :checked="props.value?.completed" />
    </td>
  </tr>
</template>

<style scoped>
td {
  border: 1px solid black;
  margin: 0;
}
</style>