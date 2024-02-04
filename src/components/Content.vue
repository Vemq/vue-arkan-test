<script setup lang="ts">
import { ref, computed } from 'vue';
import Item from "./Item.vue";

interface ItemData {
  id: number;
  title: string;
  selected: boolean;
  done: boolean;
}

const itemsState = ref<ItemData[]>([
  { id: 1, title: 'Элемент 1', selected: false, done: false },
  { id: 2, title: 'Элемент 2', selected: false, done: false },
  { id: 3, title: 'Элемент 3', selected: false, done: false }
]);

const undoneItems = computed(() => itemsState.value.filter(item => !item.done));
const doneItems = computed(() => itemsState.value.filter(item => item.done));

function switchItemSelection(id: number) {
  const clickedItem = itemsState.value.find(item => item.id === id);
  if (clickedItem) {
    clickedItem.selected = !clickedItem.selected;
  }
}

function moveSelected() {
  itemsState.value.forEach(item => {
    if (item.selected) {
      item.done = true;
    }
  })
}

function moveAll() {
  itemsState.value.forEach(item => {
    item.done = true;
  })
}

function addItem() {
  const lastItem = itemsState.value[itemsState.value.length - 1];
  const newItemId = lastItem ? lastItem.id + 1 : 1;

  itemsState.value.push({
    id: newItemId,
    title: `Элемент ${newItemId}`,
    selected: false,
    done: false
  });
}

function clearDone() {
  itemsState.value = itemsState.value.filter(item => !item.done);
}
</script>

<template>
  <div class="content">
    <div>
      <div class="items-list">
        <Item 
          v-for="item in undoneItems" 
          :key="item.id" 
          :selected="item.selected" 
          @click="switchItemSelection(item.id)">
            {{ item.title }}
        </Item>
      </div>
      <button @click="addItem">+ Добавить элемент</button>
    </div>

    <div class="buttons">
      <button @click="moveSelected">Перенести выбранные</button>
      <button @click="moveAll">Перенести все</button>
    </div>

    <div>
      <div class="items-list">
        <Item 
          v-for="item in doneItems" 
          :key="item.id" 
          :done="item.done">
            {{ item.title }}
        </Item>
      </div>
      <button @click="clearDone">X Очистить</button>
    </div>
  </div>
</template>

<style scoped>
.content {
  display: flex;
  gap: 15px;
}

.content button {
  cursor: pointer;
}

.items-list {
  display: flex;
  flex-direction: column;
  gap: 5px;
  padding: 5px;
  outline: 1px solid gainsboro;
  border-radius: 5px;
  width: 300px;
  height: 320px;
  margin-bottom: 10px;
  overflow-y: auto;
}

.buttons {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-self: center;
  gap: 10px;
}
</style>
