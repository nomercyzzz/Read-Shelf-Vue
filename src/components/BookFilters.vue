<template>
  <div class="filters">
    <div class="search-block">
      <input
        v-model="searchQuery"
        type="text"
        placeholder="Поиск по названию или автору..."
      />
    </div>

    <div class="middle-row">
      <div class="buttons">
        <button
          v-for="option in filterOptions"
          :key="option.value"
          @click="$emit('update:filter', option.value)"
          :class="['filter-btn', { active: filter === option.value }]"
        >
          {{ option.label }}
        </button>
      </div>
    </div>

    <div class="stats">
      <span>Всего: {{ total }}</span>
      <span>Прочитано: {{ completed }}</span>
      <span>Осталось: {{ total - completed }}</span>
    </div>
  </div>
</template>

<script setup>
import { computed } from 'vue'

const props = defineProps(['filter', 'books'])
defineEmits(['update:filter'])

const searchQuery = defineModel('searchQuery')

const filterOptions = [
  { value: 'all', label: 'Все' },
  { value: 'unread', label: 'Непрочитанные' },
  { value: 'read', label: 'Прочитанные' }
]

const total = computed(() => props.books.length)
const completed = computed(() => props.books.filter(book => book.completed).length)
</script>

<style scoped>
.filters {
  background: #0f0f0f;
  border: 1px solid #252525;
  border-radius: 18px;
  padding: 18px;
  margin-bottom: 16px;
  animation: fadeUp 0.3s ease-in-out;
}

.search-block input {
  width: 100%;
  background: #111111;
  color: #ffffff;
  border: 1px solid #252525;
  border-radius: 10px;
  padding: 12px 14px;
  transition: all 0.3s ease-in-out;
}

.search-block input::placeholder {
  color: #707070;
}

.search-block input:focus {
  outline: none;
  border-color: #ffffff;
  box-shadow: 0 0 0 3px rgba(255,255,255,0.08);
}

.middle-row {
  margin-top: 12px;
}

.buttons {
  display: flex;
  gap: 8px;
  flex-wrap: wrap;
}

.filter-btn {
  background: #111111;
  color: #d6d6d6;
  border: 1px solid #252525;
  border-radius: 10px;
  padding: 9px 13px;
  cursor: pointer;
  transition: all 0.3s ease-in-out;
}

.filter-btn:hover {
  border-color: #4a4a4a;
  color: #ffffff;
}

.filter-btn.active {
  background: #ffffff;
  color: #0d0d0d;
  border-color: #ffffff;
  font-weight: 700;
}

.stats {
  margin-top: 14px;
  padding-top: 14px;
  border-top: 1px solid #252525;
  display: flex;
  gap: 12px;
  flex-wrap: wrap;
  color: #9a9a9a;
  font-size: 14px;
}

@keyframes fadeUp {
  from {
    opacity: 0;
    transform: translateY(12px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
</style>