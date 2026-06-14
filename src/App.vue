<script setup>
import { ref, onMounted } from 'vue'

const items = ref([])
const isLoading = ref(false)
const error = ref(null)

const loadItems = async () => {
  isLoading.value = true
  error.value = null
  
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/posts?_limit=10')
    if (!response.ok) throw new Error(`Помилка HTTP: ${response.status}`)
    
    items.value = await response.json()
  } catch (err) {
    error.value = err.message
  } finally {
    isLoading.value = false
  }
}

onMounted(() => {
  loadItems()
})
</script>