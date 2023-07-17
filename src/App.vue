<script setup lang="ts">
import { ref } from 'vue';
import ListItem, { type Item } from './components/ListItem.vue';

const newestId = ref(0);
const randomList = ref<Item[]>([]);

function addItem() {
  const newItem = prompt("Enter new to-do item here:");
  if (newItem === null) {
    alert("Error, did not add item into to-do list.");
  } else {
    randomList.value = [...randomList.value, { value: newItem, id: ++newestId.value } ];
  }
}

function handleDelete(id : number) {
  randomList.value = randomList.value.filter((item) => item.id !== id);
}
</script>

<template>
  <h1>Simple To-do list</h1>
  <ol>
    <ListItem :value="item" :onDelete="() => handleDelete(item.id)" v-for="item in randomList" :key="item.id" />
  </ol>
  <button @click="addItem">Add new item</button>
</template>

<style scoped>

</style>
