<script setup lang="ts">
import { computed, ref } from 'vue';
import ListItem, { type Item } from './components/ListItem.vue';

const newestId = ref(0);
const toggled = ref(true);
const randomList = ref<Item[]>([]);
const showRandomList = computed(() => {
  if (!toggled.value) {
    console.log(randomList.value);
    return randomList.value.filter((item) => !item.completed) 
  } else {
    return randomList.value
  }
});

function addItem() {
  const newItem = prompt("Enter new to-do item here:");
  if (newItem === null) {
    alert("Error, did not add item into to-do list.");
  } else {
    randomList.value = [...randomList.value, { value: newItem, id: ++newestId.value, completed: false } ];
  }

console.log(showRandomList.value);
}

function toggleCompletedView() {
  toggled.value = !toggled.value;
}

function handleDelete(id : number) {
  randomList.value = randomList.value.filter((item) => item.id !== id);
}

function findId(id : number) {
  for (let i = 0; i < randomList.value.length; i++) {
    if (randomList.value[i].id === id) {
      return i;
    }
  }
  return null;
}

function handleChange(id : number) {
  const foundItem = findId(id);
  if (foundItem !== null) {
    const found = randomList.value[foundItem];
    randomList.value = [
      ...randomList.value.slice(0, foundItem),
      { ...found, completed: !found.completed },
      ...randomList.value.slice(foundItem + 1),
    ];
  }
}
</script>

<template>
  <h1>Simple To-do list</h1>
  <ol>
    <ListItem
      :value="item"
      :onDelete="() => handleDelete(item.id)"
      :onChange="() => handleChange(item.id)"
      v-for="item in showRandomList"
      :key="item.id" />
  </ol>
  <button @click="addItem">Add new item</button>
  <button @click="toggleCompletedView">Toggle view completed items</button>
</template>

<style scoped>

</style>
