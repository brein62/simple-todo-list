<script setup lang="ts">
import { computed, ref } from 'vue';
import ListItem, { type Item } from './components/ListItem.vue';

const newestId = ref(getCount());
const toggled = ref(true);
const randomList = ref<Item[]>(getList() as Item[]);
const showRandomList = computed(() => {
  if (!toggled.value) {
    console.log(randomList.value);
    return randomList.value.filter((item) => !item.completed) 
  } else {
    return randomList.value
  }
});

function setList() {
  window.localStorage.setItem("list", JSON.stringify(randomList.value));
  window.localStorage.setItem("count", newestId.toString());
}

function getCount() {
  if (window.localStorage.getItem("count") === null) {
    return 0;
  } else {
    return parseInt(window.localStorage.getItem("count") ?? "0");
  }
}

function getList() {
  if (window.localStorage.getItem("list") === null) {
    return [];
  } else {
    return JSON.parse(window.localStorage.getItem("list") ?? "[]");
  }
}

function addItem() {
  const newItem = prompt("Enter new to-do item here:");
  if (newItem === null) {
    alert("Error, did not add item into to-do list.");
  } else {
    randomList.value = [...randomList.value, { value: newItem, id: ++newestId.value, completed: false } ];
    setList();
  }

console.log(showRandomList.value);
}

function toggleCompletedView() {
  toggled.value = !toggled.value;
}

function handleDelete(id : number) {
  randomList.value = randomList.value.filter((item) => item.id !== id);
  setList();
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
  setList();
}

function clearCompletedItems() {
  randomList.value = randomList.value.filter((item) => !item.completed);
  setList();
}

function clearItems() {
  randomList.value = [];
  newestId.value = 0;
  setList();
}
</script>

<template>
  <main>
    <h1>Simple To-do list</h1>
    <table :style="{border: '1px solid black'}">
      <thead>
        <th>ID</th>
        <th>Item</th>
        <th colspan="2">Actions</th>
      </thead>
      <tbody>
        <ListItem
          :value="item"
          :onDelete="() => handleDelete(item.id)"
          :onChange="() => handleChange(item.id)"
          v-for="item in showRandomList"
          :key="item.id" />
      </tbody>
    </table>
    <div>
      <button @click="addItem">Add new item</button>
    </div>
    <div>
      <button @click="toggleCompletedView">Toggle view completed items</button>
    </div>
    <div>
      <button @click="clearCompletedItems">Clear completed items</button>
    </div>
    <div>
      <button @click="clearItems">Clear items</button>
    </div>
  </main>
</template>

<style scoped>
main {
  font-family: system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
  text-align: center;
}

table {
  margin: 0 auto;
}

table tr:nth-child(odd) {
  background-color:lightblue;
}

table tr:nth-child(even) {
  background-color: rgb(134, 199, 221);
}
</style>
