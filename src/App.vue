<template>
  <div v-if="!selectedItem" style="margin-bottom: 20px;">
      <input v-model="searchQuery" type="text" placeholder="Пошук за заголовком..." />
    </div>

    <ul v-else-if="filteredItems.length > 0">
      <li v-for="item in filteredItems" :key="item.id">
         </li>
    </ul>
    
    <div v-else-if="!selectedItem && filteredItems.length === 0" class="empty">
      Нічого не знайдено
    </div>
  </template>

<script setup>
import { ref, computed, onMounted } from 'vue'

const searchQuery = ref('')

const filteredItems = computed(() => {
  if (!searchQuery.value) return items.value
  return items.value.filter(item => 
    item.title.toLowerCase().includes(searchQuery.value.toLowerCase())
  )
})
</script>