<template>
  <div class="book-card" :class="{ completed: book.completed }">
    <div class="book-main">
      <div class="book-meta">
        <span class="genre">{{ book.genre }}</span>
      </div>

      <h3>{{ book.title }}</h3>
      <p class="author">{{ book.author }}</p>

      <div v-if="book.completed" class="rating">
        <span
          v-for="star in 5"
          :key="star"
          @click="$emit('rate', star)"
        >
          {{ star <= book.rating ? '★' : '☆' }}
        </span>
      </div>
    </div>

    <div class="book-actions">
      <button
        @click="$emit('toggle')"
        :class="['btn', book.completed ? 'btn-secondary' : 'btn-primary']"
      >
        {{ book.completed ? 'Прочитано' : 'Отметить прочитанной' }}
      </button>

      <button @click="$emit('delete')" class="btn btn-danger">
        Удалить
      </button>
    </div>
  </div>
</template>

<script setup>
defineProps(['book'])
defineEmits(['toggle', 'delete', 'rate'])
</script>

<style scoped>
.book-card {
  background: #111111;
  border: 1px solid #252525;
  border-radius: 16px;
  padding: 16px;
  margin-bottom: 10px;
  display: flex;
  justify-content: space-between;
  gap: 16px;
  transition: all 0.3s ease-in-out;
  animation: fadeUp 0.3s ease-in-out;
}

.book-card:hover {
  border-color: #3a3a3a;
  transform: translateY(-2px);
  background: #141414;
}

.book-card.completed {
  border-color: #4a4a4a;
}

.book-main {
  flex: 1;
}

.book-meta {
  display: flex;
  gap: 8px;
  margin-bottom: 8px;
}

.genre {
  display: inline-flex;
  align-items: center;
  padding: 4px 8px;
  border-radius: 999px;
  border: 1px solid #252525;
  background: rgba(255,255,255,0.03);
  color: #9a9a9a;
  font-size: 12px;
}

.book-main h3 {
  color: #ffffff;
  font-size: 19px;
  margin-bottom: 4px;
  line-height: 1.25;
}

.author {
  color: #9a9a9a;
  margin-bottom: 10px;
}

.rating {
  display: flex;
  gap: 4px;
}

.rating span {
  color: #ffffff;
  font-size: 20px;
  cursor: pointer;
  transition: all 0.3s ease-in-out;
}

.rating span:hover {
  transform: scale(1.15);
  opacity: 0.8;
}

.book-actions {
  display: flex;
  flex-direction: column;
  gap: 8px;
  min-width: 170px;
}

.btn {
  border: 1px solid #252525;
  border-radius: 10px;
  padding: 11px 13px;
  cursor: pointer;
  transition: all 0.3s ease-in-out;
  font-weight: 600;
}

.btn-primary {
  background: #ffffff;
  color: #0d0d0d;
  border-color: #ffffff;
}

.btn-primary:hover {
  opacity: 0.88;
}

.btn-secondary {
  background: #181818;
  color: #ffffff;
}

.btn-secondary:hover {
  border-color: #4a4a4a;
}

.btn-danger {
  background: transparent;
  color: #d8d8d8;
}

.btn-danger:hover {
  background: #1a1a1a;
  color: #ffffff;
  border-color: #4a4a4a;
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