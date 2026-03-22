<template>
  <div class="app">
    <header class="hero">
      <h1>ReadShelf</h1>
    </header>

    <main class="main-card">
      <AddBookForm @add-book="addBook" />

      <BookFilters
        v-model:searchQuery="searchQuery"
        :filter="currentFilter"
        @update:filter="currentFilter = $event"
        :books="books"
      />

      <div v-if="filteredBooks.length === 0" class="empty-state">
        <p>Книги не найдены</p>
        <p>Добавь первую книгу или измени параметры поиска</p>
      </div>

      <div v-else class="books-list">
        <BookCard
          v-for="book in filteredBooks"
          :key="book.id"
          :book="book"
          @toggle="toggleBook(book.id)"
          @delete="deleteBook(book.id)"
          @rate="rateBook(book.id, $event)"
        />
      </div>
    </main>
  </div>
</template>

<script setup>
import { ref, computed, watch } from 'vue'
import AddBookForm from './components/AddBookForm.vue'
import BookFilters from './components/BookFilters.vue'
import BookCard from './components/BookCard.vue'

const books = ref([])
const currentFilter = ref('all')
const searchQuery = ref('')

const savedBooks = localStorage.getItem('books')
if (savedBooks) {
  try {
    books.value = JSON.parse(savedBooks)
  } catch (error) {
    console.error('Ошибка чтения localStorage:', error)
    books.value = []
  }
}

watch(
  books,
  (newBooks) => {
    localStorage.setItem('books', JSON.stringify(newBooks))
  },
  { deep: true }
)

const addBook = (bookData) => {
  const newBook = {
    id: Date.now(),
    ...bookData,
    completed: false,
    rating: 0
  }

  books.value.push(newBook)
}

const toggleBook = (id) => {
  const book = books.value.find(b => b.id === id)

  if (book) {
    book.completed = !book.completed

    if (!book.completed) {
      book.rating = 0
    }
  }
}

const rateBook = (id, rating) => {
  const book = books.value.find(b => b.id === id)

  if (book && book.completed) {
    book.rating = rating
  }
}

const deleteBook = (id) => {
  if (confirm('Удалить книгу?')) {
    books.value = books.value.filter(b => b.id !== id)
  }
}

const filteredBooks = computed(() => {
  return books.value
    .filter(book => {
      if (currentFilter.value === 'unread') return !book.completed
      if (currentFilter.value === 'read') return book.completed
      return true
    })
    .filter(book => {
      if (!searchQuery.value) return true

      const query = searchQuery.value.toLowerCase()

      return (
        book.title.toLowerCase().includes(query) ||
        book.author.toLowerCase().includes(query)
      )
    })
})
</script>

<style>
:root {
  --bg: #0d0d0d;
  --surface: #111111;
  --surface-2: #161616;
  --border: #252525;
  --border-hover: #3a3a3a;
  --text: #ffffff;
  --muted: #9a9a9a;
  --muted-2: #6f6f6f;
  --accent: #ffffff;
  --shadow: 0 10px 30px rgba(0, 0, 0, 0.35);
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html, body, #app {
  min-height: 100%;
}

body {
  font-family: Inter, Segoe UI, Arial, sans-serif;
  background: var(--bg);
  color: var(--text);
  line-height: 1.6;
}

button,
input,
select {
  font: inherit;
  transition: all 0.3s ease-in-out;
}

.app {
  min-height: 100vh;
  max-width: 780px;
  margin: 0 auto;
  padding: 28px 18px 40px;
}

.hero {
  text-align: center;
  margin-bottom: 18px;
  animation: fadeUp 0.3s ease-in-out;
}

.hero h1 {
  font-size: 38px;
  line-height: 1.1;
  letter-spacing: -0.03em;
}

.main-card {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: 22px;
  padding: 20px;
  box-shadow: var(--shadow);
  animation: fadeUp 0.3s ease-in-out;
}

.empty-state {
  text-align: center;
  padding: 44px 20px;
  color: var(--muted);
  border: 1px dashed var(--border);
  border-radius: 16px;
  background: #0f0f0f;
  animation: fadeUp 0.3s ease-in-out;
}

.empty-icon {
  font-size: 40px;
  color: var(--text);
  margin-bottom: 10px;
}

.empty-state p:nth-child(2) {
  color: var(--text);
  font-size: 18px;
  margin-bottom: 6px;
}

.books-list {
  margin-top: 16px;
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