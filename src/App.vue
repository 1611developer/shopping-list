<script setup>
import { ref, computed } from 'vue'

const header = ref('Shopping List App')
const characterCount = computed(() => {
  return newItem.value.length
})
const editing = ref(false)
const items = ref([
  {id: 1, label: "carton of eggs", purchased: true, highPriority: false},
  {id: 2, label:"2% milk", purchased: true, highPriority: false},
  {id: 3, label: "strawberries", purchased: false, highPriority: true}
])
const newItem = ref("")
const newItemHighPriority = ref(false)
const saveItem = () => {
  items.value.push(
  {
  id: items.value.length + 1,
  label: newItem.value,
  purchased: false,
  highPriority: newItemHighPriority.value
  })
  // reset the form to empty
  newItem.value = ""
  // reset the priority to unchecked
  newItemHighPriority.value = false
}

// e is short for editing
const doEdit = (e) => {
  editing.value = e
  newItem.value = ''
  // reset the priority to unchecked
  newItemHighPriority.value = false
}

const togglePurchased = (item) => {
  item.purchased = !item.purchased
}

// use spread operator so not to alter original array
const reversedItems = computed(() => [...items.value].reverse())

</script>

<template>
  <div class="header">
    <h1 >{{ header }}</h1>
    <button v-if="editing" class="btn" @click="doEdit(false)">Cancel</button>
    <button v-else class="btn btn-primary" @click="doEdit(true)">Add Item</button> 
  </div>
  <form class="add-it em-form"
  v-if="editing"
  @submit.prevent="saveItem">
    <input 
    	v-model.trim="newItem"
 			type="text" 
      placeholder="Add an item"
    >
    <label>
      <input type="checkbox" v-model="newItemHighPriority"> 
      High Priority
    </label>
    <button
      v-bind:disabled="newItem.length < 4 || newItem.length > 40  "
      class="btn btn-primary">
      Save Item
    </button>
  </form>
  <p
    class="counter"
    v-if="editing"
    >
    {{ characterCount }}/40
  </p>
  
  <ul>
    <li
      v-for="(item, index) in reversedItems"
      @click="togglePurchased(item)"
      class="static-class"
      :key="item.id"
      :class="{strikeout: item.purchased, priority: item.highPriority}"
    >
      {{item.label}}
    </li>
  </ul>

  <p v-if="!items.length">
    Nothing to show here.
  </p>
</template>