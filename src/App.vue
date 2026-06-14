<template>
  <div class="app-container">
    <h1>Каталог публікацій</h1>

    <div v-if="isLoading" class="loading">Завантаження даних...</div>
    <div v-else-if="error" class="error">Помилка: {{ error }}</div>
    
    <div v-else-if="selectedItem" class="details-view">
      <button @click="selectedItem = null">← Назад до списку</button>
      <h2>{{ selectedItem.title }}</h2>
      <p>{{ selectedItem.body }}</p>
    </div>

    <ul v-else-if="items.length > 0">
      <li v-for="item in items" :key="item.id" style="margin-bottom: 10px;">
        <strong>{{ item.title }}</strong>
        <button @click="selectedItem = item" style="margin-left: 10px;">Деталі</button>
      </li>
    </ul>
    
    <div v-else class="empty">Немає даних</div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const items = ref([])
const isLoading = ref(false)
const error = ref(null)
const selectedItem = ref(null) 

</script>