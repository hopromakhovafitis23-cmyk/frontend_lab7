<template>
  <div class="app-container">
    <h1>Каталог публікацій (API)</h1>

    <div v-if="isLoading" class="loading">Завантаження даних...</div>
    <div v-else-if="error" class="error">Помилка: {{ error }}</div>
    <div v-else-if="items.length === 0" class="empty">Немає даних</div>
    
    <div v-else>
      </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const items = ref([])
const isLoading = ref(false)
const error = ref(null)
</script>

<style scoped>
.app-container { max-width: 800px; margin: 0 auto; font-family: sans-serif; }
.loading { color: blue; }
.error { color: red; }
.empty { color: gray; }
</style>