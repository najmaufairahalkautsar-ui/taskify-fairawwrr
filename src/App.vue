<script setup>
import { ref, computed } from 'vue'

const tasks = ref([
  { id: 1, title: 'tugas mtk', status: false },
  { id: 2, title: 'tugas sejarah', status: true }
])
const inputTask = ref('')

// Computed untuk statistik tugas
const totalTasks = computed(() => tasks.value.length)
const completedTasks = computed(() => tasks.value.filter(task => task.status).length)
const remainingTasks = computed(() => totalTasks.value - completedTasks.value)

// Persentase progres untuk progress bar
const progressPercentage = computed(() => {
  if (totalTasks.value === 0) return 0
  return Math.round((completedTasks.value / totalTasks.value) * 100)
})

const addTask = (title) => {
  if (!title.trim()) return

  const newTask = {
    id: new Date().getTime(),
    title: title,
    status: false
  }

  tasks.value.push(newTask)
  inputTask.value = ''
}

const deleteTask = (id) => {
  const task = tasks.value.find(item => item.id === id)

  if (!task) {
    alert('Task tidak ditemukan 🌸')
    return
  }

  const confirmation = confirm('Apakah kamu yakin ingin menghapus tugas ini? 🥺')

  if (confirmation) {
    tasks.value = tasks.value.filter(item => item.id !== id)
  }
}
</script>

<template>
  <main class="page">
    <!-- Dekorasi latar belakang (Awan & Bintang Pixel) -->
    <div class="decor cloud cloud-left">☁️</div>
    <div class="decor cloud cloud-right">☁️</div>
    <div class="decor star star-1">✦</div>
    <div class="decor star star-2">✧</div>
    <div class="decor star star-3">✦</div>
    <div class="decor star star-4">✧</div>

    <div class="task-box">
      <!-- Stiker/Maskot Lucu -->
      <div class="cute-badge">🐰 ✨</div>
      
      <h1>Taskify Vue</h1>

      <!-- Form Input -->
      <form @submit.prevent="addTask(inputTask)">
        <div class="input-wrapper">
          <input
            v-model="inputTask"
            type="text"
            placeholder="Masukkan tugas baru..."
            aria-label="Tugas baru"
          />
        </div>

        <button type="submit" class="add-button">
          <span class="plus">➕</span>
          Tambah
        </button>
      </form>

      <!-- Panel Statistik (Total, Sisa, Selesai) -->
      <div class="stats-panel">
        <div class="stat-card">
          <span class="stat-label">📋 Total</span>
          <span class="stat-value">{{ totalTasks }}</span>
        </div>
        <div class="stat-card remaining">
          <span class="stat-label">⏳ Sisa</span>
          <span class="stat-value">{{ remainingTasks }}</span>
        </div>
        <div class="stat-card completed">
          <span class="stat-label">✅ Selesai</span>
          <span class="stat-value">{{ completedTasks }}</span>
        </div>
      </div>

      <!-- Progress Bar Imut -->
      <div class="progress-container">
        <div class="progress-bar-bg">
          <div class="progress-bar-fill" :style="{ width: progressPercentage + '%' }"></div>
        </div>
        <span class="progress-text">{{ progressPercentage }}% Selesai 🍓</span>
      </div>

      <!-- Daftar Tugas -->
      <ul class="task-list">
        <li
          v-for="task in tasks"
          :key="task.id"
          class="task-item"
        >
          <label class="task-name">
            <input
              v-model="task.status"
              type="checkbox"
              class="custom-checkbox"
            />
            <span :class="{ completed: task.status }">
              {{ task.title }}
            </span>
          </label>

          <button
            class="delete-button"
            type="button"
            @click="deleteTask(task.id)"
          >
            <svg
              width="16"
              height="16"
              viewBox="0 0 24 24"
              fill="none"
              stroke="currentColor"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round"
            >
              <path d="M3 6h18M8 6V4h8v2M19 6l-1 14H6L5 6" />
              <path d="M10 11v5M14 11v5" />
            </svg>
            Hapus
          </button>
        </li>

        <li v-if="tasks.length === 0" class="empty-state">
          Belum ada tugas nih, yuk tambah tugas baru! 🎀
        </li>
      </ul>
    </div>
  </main>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Comfortaa:wght@400;600;700&display=swap');

* {
  box-sizing: border-box;
}

body {
  margin: 0;
  min-height: 100vh;
  background: #fff8f8;
  font-family: 'Comfortaa', cursive, sans-serif;
  color: #63535d;
}

.page {
  position: relative;
  min-height: 100vh;
  padding: 60px 20px;
  display: flex;
  justify-content: center;
  align-items: flex-start;
  background: radial-gradient(circle at center, #ffffff 0%, #ffeaf0 100%);
  overflow: hidden;
}

/* Dekorasi Awan dan Bintang */
.decor {
  position: absolute;
  pointer-events: none;
  user-select: none;
}

.cloud {
  font-size: 40px;
  opacity: 0.5;
  filter: drop-shadow(0 2px 4px rgba(243, 177, 200, 0.4));
}
.cloud-left { top: 120px; left: 8%; }
.cloud-right { top: 90px; right: 8%; }

.star {
  color: #e5b3cb;
  font-size: 22px;
  animation: pulse 2s infinite alternate;
}
.star-1 { top: 100px; left: 18%; }
.star-2 { top: 70px; right: 18%; }
.star-3 { top: 220px; right: 10%; }
.star-4 { top: 200px; left: 10%; }

@keyframes pulse {
  from { opacity: 0.4; transform: scale(0.9); }
  to { opacity: 0.9; transform: scale(1.1); }
}

/* Kotak Utama */
.task-box {
  position: relative;
  width: 100%;
  max-width: 650px;
  padding: 35px 35px 25px;
  background: #fde2eb;
  border: 3px solid #f3b1c8;
  border-radius: 24px;
  box-shadow: 
    0 0 0 5px #faedf2,
    0 0 0 8px #f3b1c8,
    0 15px 35px rgba(224, 150, 178, 0.25);
  z-index: 1;
}

/* Badge Maskot Lucu */
.cute-badge {
  position: absolute;
  top: -20px;
  right: 25px;
  background: #ffffff;
  border: 2px solid #f3b1c8;
  padding: 4px 12px;
  border-radius: 20px;
  font-size: 18px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.05);
}

.task-box h1 {
  margin: 0 0 25px;
  text-align: center;
  font-size: 34px;
  font-weight: 700;
  color: #554852;
  letter-spacing: 0.5px;
}

/* Form Input & Tombol Tambah */
.task-box form {
  display: flex;
  gap: 12px;
  margin-bottom: 20px;
}

.input-wrapper {
  flex: 1;
  background: #fffdf6;
  border: 2px solid #e2a8be;
  border-radius: 12px;
  padding: 3px;
  box-shadow: inset 0 2px 4px rgba(0, 0, 0, 0.03);
}

.task-box form input[type="text"] {
  width: 100%;
  height: 100%;
  padding: 12px 16px;
  border: 1px solid #f1c7d6;
  border-radius: 8px;
  background: transparent;
  color: #63535d;
  font-family: inherit;
  font-size: 16px;
  outline: none;
}

.task-box form input[type="text"]::placeholder {
  color: #a894a0;
}

.add-button {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 6px;
  padding: 0 20px;
  height: 48px;
  border: 2px solid #a35079;
  border-radius: 12px;
  background: #c86897;
  color: #ffffff;
  font-family: inherit;
  font-size: 16px;
  font-weight: 600;
  cursor: pointer;
  box-shadow: inset 0 2px 0 rgba(255, 255, 255, 0.3), 0 3px 0 #923f68;
  transition: all 0.15s ease;
}

.add-button:hover {
  background: #b85b8a;
}

.add-button:active {
  transform: translateY(2px);
  box-shadow: inset 0 2px 0 rgba(255, 255, 255, 0.3), 0 1px 0 #923f68;
}

/* Panel Statistik (Total, Sisa, Selesai) */
.stats-panel {
  display: flex;
  justify-content: space-between;
  gap: 10px;
  margin-bottom: 15px;
}

.stat-card {
  flex: 1;
  background: rgba(255, 255, 255, 0.7);
  border: 2px solid #f1c7d6;
  border-radius: 12px;
  padding: 8px 12px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.stat-card.remaining {
  background: #fff8eb;
  border-color: #f7d2a9;
}

.stat-card.completed {
  background: #f0fbf5;
  border-color: #b7e4ca;
}

.stat-label {
  font-size: 12px;
  color: #7b6d76;
  font-weight: 600;
}

.stat-value {
  font-size: 18px;
  font-weight: 700;
  color: #554852;
}

/* Progress Bar */
.progress-container {
  margin-bottom: 20px;
  text-align: right;
}

.progress-bar-bg {
  width: 100%;
  height: 10px;
  background: #f7d1df;
  border-radius: 10px;
  overflow: hidden;
  border: 1px solid #e2a8be;
}

.progress-bar-fill {
  height: 100%;
  background: linear-gradient(90deg, #e685b5, #c86897);
  border-radius: 10px;
  transition: width 0.3s ease;
}

.progress-text {
  font-size: 12px;
  color: #8c7182;
  margin-top: 4px;
  display: inline-block;
  font-weight: 600;
}

/* List Tugas */
.task-list {
  list-style: none;
  padding: 0;
  margin: 0;
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.task-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 14px 18px;
  background: #fff8fb;
  border: 2px solid #f3c2d4;
  border-radius: 14px;
  box-shadow: 0 2px 6px rgba(224, 150, 178, 0.12);
  transition: transform 0.15s ease, box-shadow 0.15s ease;
}

.task-item:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 10px rgba(224, 150, 178, 0.2);
}

.task-name {
  display: flex;
  align-items: center;
  gap: 12px;
  cursor: pointer;
  user-select: none;
  font-size: 16px;
}

/* Custom Checkbox */
.custom-checkbox {
  appearance: none;
  width: 22px;
  height: 22px;
  border: 2px solid #c86897;
  border-radius: 6px;
  background: #fff;
  cursor: pointer;
  display: grid;
  place-content: center;
  transition: all 0.2s ease;
}

.custom-checkbox::before {
  content: "✔";
  font-size: 13px;
  color: white;
  transform: scale(0);
  transition: transform 0.15s ease-in-out;
}

.custom-checkbox:checked {
  background: #c86897;
}

.custom-checkbox:checked::before {
  transform: scale(1);
}

.completed {
  text-decoration: line-through;
  color: #b59ea9;
}

/* Tombol Hapus */
.delete-button {
  display: flex;
  align-items: center;
  gap: 4px;
  background: transparent;
  border: none;
  color: #c86897;
  font-family: inherit;
  font-size: 14px;
  font-weight: 600;
  cursor: pointer;
  padding: 6px 10px;
  border-radius: 8px;
  transition: background 0.15s ease, color 0.15s ease;
}

.delete-button:hover {
  background: #fde8f0;
  color: #a33f70;
}

.empty-state {
  text-align: center;
  padding: 20px;
  color: #a894a0;
  font-size: 14px;
  font-style: italic;
}
</style>