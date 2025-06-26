<script setup>
import { ref } from "vue";
import { leftItems, rightItems } from "./mock/mockData";

const selectedItemsLeft = ref([]);
const selectedItemsRight = ref(null);

const handleSelectionMultiple = (item) => {
  const isSelected = selectedItemsLeft.value.some(
    (selectedItem) => selectedItem.id === item.id
  );

  if (isSelected) {
    selectedItemsLeft.value = selectedItemsLeft.value.filter(
      (selectedItem) => selectedItem.id !== item.id
    );
  } else {
    selectedItemsLeft.value.push(item);
  }
};

const handleSelectionSingle = (item) => {
  if (selectedItemsRight.value?.id === item.id) {
    selectedItemsRight.value = null;
  } else {
    selectedItemsRight.value = item;
  }
};

const isItemSelected = (item, selectedItems) => {
  return Array.isArray(selectedItems)
    ? selectedItems.some((selected) => selected.id === item.id)
    : selectedItems?.id === item.id;
};
</script>

<template>
  <div class="app">
    <section class="display-row">
      <article class="item-container item-container--multiple">
        <div class="item-container__items">
          <div
            v-for="item in selectedItemsLeft"
            :key="item.id"
            class="item-container__item"
          >
            {{ item.name }}
          </div>
        </div>
        <p class="item-container__counter">
          Selected: {{ selectedItemsLeft.length }}/{{ leftItems.length }}
        </p>
      </article>

      <article class="item-container item-container--single">
        <span class="item-container__content">
          {{ selectedItemsRight?.name || "No item selected" }}
        </span>
      </article>
    </section>

    <section class="selection-row">
      <article class="item-container item-container--selectable">
        <div
          v-for="item in leftItems"
          :key="item.id"
          class="item-container__item item-container__item--clickable"
          :class="{
            'item-container__item--selected': isItemSelected(
              item,
              selectedItemsLeft
            ),
          }"
          @click="handleSelectionMultiple(item)"
        >
          {{ item.name }}
        </div>
      </article>

      <article class="item-container item-container--selectable">
        <div
          v-for="item in rightItems"
          :key="item.id"
          class="item-container__item item-container__item--clickable"
          :class="{
            'item-container__item--selected': isItemSelected(
              item,
              selectedItemsRight
            ),
          }"
          @click="handleSelectionSingle(item)"
        >
          {{ item.name }}
        </div>
      </article>
    </section>
  </div>
</template>

<style scoped>
.app {
  padding: 20px;
}

.display-row,
.selection-row {
  display: flex;
  flex-direction: row;
  gap: 20px;
  justify-content: space-between;
  align-items: flex-start;
  margin-bottom: 20px;
}

.item-container {
  border: 3px solid #000;
  padding: 20px;
  display: flex;
  flex-direction: column;
  gap: 15px;
  min-height: 100px;
}

.item-container--multiple {
  max-width: 40%;
  min-width: 200px;
  justify-content: flex-start;
  align-items: flex-start;
}

.item-container--single {
  max-width: 40%;
  min-width: 200px;
  justify-content: center;
  align-items: center;
}

.item-container--selectable {
  flex-direction: row;
  flex-wrap: wrap;
  width: 45%;
  justify-content: flex-start;
  align-items: center;
}

.item-container__items {
  display: flex;
  flex-direction: row;
  gap: 10px;
  flex-wrap: wrap;
}

.item-container__item {
  border: 2px solid #333;
  padding: 12px 16px;
  text-align: center;
  border-radius: 4px;
  background-color: #fff;
  transition: all 0.2s ease;
}

.item-container__item--clickable {
  cursor: pointer;
  user-select: none;
}

.item-container__item--clickable:hover {
  background-color: #f0f0f0;
  border-color: #666;
}

.item-container__item--selected {
  border-color: #e74c3c;
  background-color: #ffeaea;
  color: #c0392b;
}

.item-container__counter {
  margin: 0;
  font-weight: 600;
  color: #666;
  font-size: 14px;
}

.item-container__content {
  font-size: 16px;
  color: #333;
  text-align: center;
}
</style>
