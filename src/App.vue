<script setup lang="ts">
import { ref, watch } from 'vue'  

import TheHeader from './components/TheHeader.vue'
import SearchInput from './components/SearchInput.vue'
import DrugCard from './components/DrugCard.vue'
import AppSpinner from './components/AppSpinner.vue'

interface Drug {
  id: number;
  generic_name: string;
  dosage_forms: string[];
  ed_level: string | null;
}

const searchQuery = ref('')
const searchResults = ref<Drug[]>([])
const isLoading = ref(false)
const error = ref<string | null>(null)
const hasSearched = ref(false)
let debounceTimer: number

const performSearch = () => {
  if (searchQuery.value.trim().length < 2) {
    searchResults.value = []
    hasSearched.value = false
    return
  }
  
  isLoading.value = true
  hasSearched.value = true
  error.value = null

  // Use an any-typed fetch chain to avoid relying on the TS Promise lib when targeting ES5
  ;(window as any).fetch(`https://thai-nlem-api.onrender.com/api/drugs/search?q=${searchQuery.value}`)
    .then((response: any) => {
      if (!response.ok) {
        throw new Error('เกิดข้อผิดพลาดในการเชื่อมต่อกับเซิร์ฟเวอร์')
      }
      return response.json()
    })
    .then((data: any) => {
      searchResults.value = data
      isLoading.value = false
    })
    .catch((err: any) => {
      error.value = (err as Error).message
      searchResults.value = []
      isLoading.value = false
    })
}

watch(searchQuery, () => {
  clearTimeout(debounceTimer)
  debounceTimer = setTimeout(performSearch, 500) as unknown as number
})
</script>

<template>
  <main>
    <TheHeader />
    
    <SearchInput v-model="searchQuery" />

    <div class="results-container">
      <AppSpinner v-if="isLoading" />

      <div v-else-if="error" class="message-display error">
        {{ error }}
      </div>

      <div v-else-if="searchResults.length === 0 && hasSearched" class="message-display">
        <h3>ไม่พบข้อมูล</h3>
        <p>ไม่พบข้อมูลยาที่ตรงกับ "{{ searchQuery }}"</p>
      </div>

      <div v-else class="results-list">
        <DrugCard
          v-for="drug in searchResults"
          :key="drug.id"
          :drug="drug"
        />
      </div>
    </div>
  </main>
</template>

<style scoped>
.results-container {
  margin-top: var(--spacing-8);
  min-height: 300px;
}
.results-list {
  display: grid;
  gap: var(--spacing-4);
}
.message-display {
  text-align: center;
  padding: var(--spacing-8) 0;
  color: var(--color-text-secondary);
}
.message-display h3 {
  font-size: 1.25rem;
  margin: 0 0 var(--spacing-2) 0;
}
.message-display p {
  margin: 0;
}
.message-display.error {
  color: var(--color-error);
  background-color: rgba(220, 53, 69, 0.1);
  border: 1px solid rgba(220, 53, 69, 0.2);
  border-radius: var(--border-radius);
  padding: var(--spacing-4);
}
</style>