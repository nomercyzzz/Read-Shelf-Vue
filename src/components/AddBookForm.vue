<template>
  <form @submit.prevent="handleSubmit" class="add-form">
    <div class="section-head">
      <h2>Добавить книгу</h2>
    </div>

    <div class="form-grid">
      <div class="form-group">
        <input
          v-model="formData.title"
          type="text"
          placeholder="Название книги"
          required
        />
      </div>

      <div class="form-group">
        <input
          v-model="formData.author"
          type="text"
          placeholder="Автор"
          required
        />
      </div>

      <div class="form-group">
        <select v-model="formData.genre" required>
          <option value="">Выберите жанр</option>
          <option value="Роман">Роман</option>
          <option value="Фантастика">Фантастика</option>
          <option value="Детектив">Детектив</option>
          <option value="Научная">Научная</option>
          <option value="Поэзия">Поэзия</option>
        </select>
      </div>
    </div>

    <button type="submit" class="btn-submit">Добавить книгу</button>
  </form>
</template>

<script setup>
import { reactive } from 'vue'

const emit = defineEmits(['add-book'])

const formData = reactive({
  title: '',
  author: '',
  genre: ''
})

const handleSubmit = () => {
  emit('add-book', {
    title: formData.title.trim(),
    author: formData.author.trim(),
    genre: formData.genre
  })

  formData.title = ''
  formData.author = ''
  formData.genre = ''
}
</script>

<style scoped>
.add-form {
  background: #0f0f0f;
  border: 1px solid #252525;
  border-radius: 18px;
  padding: 18px;
  margin-bottom: 16px;
  animation: fadeUp 0.3s ease-in-out;
}

.section-head {
  margin-bottom: 14px;
}

.section-head h2 {
  color: #ffffff;
  font-size: 20px;
  font-weight: 700;
}

.form-grid {
  display: grid;
  gap: 10px;
}

.form-group input,
.form-group select {
  width: 100%;
  background: #111111;
  color: #ffffff;
  border: 1px solid #252525;
  border-radius: 10px;
  padding: 12px 14px;
  transition: all 0.3s ease-in-out;
}

.form-group input::placeholder {
  color: #707070;
}

.form-group input:focus,
.form-group select:focus {
  outline: none;
  border-color: #ffffff;
  box-shadow: 0 0 0 3px rgba(255,255,255,0.08);
}

.btn-submit {
  width: 100%;
  margin-top: 12px;
  padding: 12px 16px;
  border: none;
  border-radius: 10px;
  background: #ffffff;
  color: #0d0d0d;
  font-weight: 700;
  cursor: pointer;
  transition: all 0.3s ease-in-out;
}

.btn-submit:hover {
  opacity: 0.88;
}

.btn-submit:active {
  transform: scale(0.98);
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