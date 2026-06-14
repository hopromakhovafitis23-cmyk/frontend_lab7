<template>
  <div class="app-container">
    <h1>Каталог публікацій</h1>

    <div v-if="!selectedItem" style="margin-bottom: 20px; display: flex; gap: 10px;">
      <input v-model="searchQuery" type="text" placeholder="Пошук..." />
      
      <button @click="changePage(-1)" :disabled="page === 1 || isLoading">Попередня</button>
      <span>Сторінка: {{ page }}</span>
      <button @click="changePage(1)" :disabled="isLoading">Наступна</button>
    </div>

    <div v-if="isLoading" class="loading">Завантаження даних...</div>
    <div v-else-if="error" class="error">Помилка: {{ error }}</div>
    
    <div v-else-if="selectedItem" class="details-view">
      <button @click="selectedItem = null">← Назад</button>
      <h2>{{ selectedItem.title }}</h2>
      <p>{{ selectedItem.body }}</p>
    </div>

    <ul v-else-if="filteredItems.length > 0">
      <li v-for="item in filteredItems" :key="item.id" style="margin-bottom: 10px;">
        <strong>{{ item.title }}</strong>
        <button @click="selectedItem = item" style="margin-left: 10px;">Деталі</button>
      </li>
    </ul>
    
    <div v-else-if="!selectedItem" class="empty">Нічого не знайдено</div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'

const items = ref([])
const isLoading = ref(false)
const error = ref(null)
const selectedItem = ref(null)
const searchQuery = ref('')

const page = ref(1)
const limit = ref(5)
let abortController = null 

const loadItems = async () => {
  if (abortController) {
    abortController.abort()
  }
  
  abortController = new AbortController()
  isLoading.value = true
  error.value = null
  
  try {
    const response = await fetch(
      `https://jsonplaceholder.typicode.com/posts?_page=${page.value}&_limit=${limit.value}`,
      { signal: abortController.signal }
    )
    if (!response.ok) throw new Error(`Помилка HTTP: ${response.status}`)
    
    items.value = await response.json()
  } catch (err) {
    if (err.name !== 'AbortError') {
      error.value = err.message
    }
  } finally {
    if (!abortController.signal.aborted) {
      isLoading.value = false
    }
  }
}

const changePage = (step) => {
  page.value += step
  loadItems()
}

const filteredItems = computed(() => {
  if (!searchQuery.value) return items.value
  return items.value.filter(item => 
    item.title.toLowerCase().includes(searchQuery.value.toLowerCase())
  )
})

onMounted(() => {
  loadItems()
})
</script>