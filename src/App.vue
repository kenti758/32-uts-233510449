<script setup>
import { ref, computed } from "vue"

const activities = ref([
  { id: 1, name: "Meeting with team", done: false },
  { id: 2, name: "Design presentation", done: true },
])

const newActivity = ref("")
const filterStatus = ref("all")

const addActivity = () => {
  const name = newActivity.value.trim()
  if (name) {
    activities.value.unshift({
      id: Date.now(),
      name,
      done: false,
    })
    newActivity.value = ""
  }
}

const removeActivity = (id) => {
  activities.value = activities.value.filter(a => a.id !== id)
}

const filteredActivities = computed(() => {
  if (filterStatus.value === "done") return activities.value.filter(a => a.done)
  if (filterStatus.value === "undone") return activities.value.filter(a => !a.done)
  return activities.value
})
</script>

<template>
  <div class="wrapper">
    <header>
      <h1><span>Kegiatan</span> - Harian</h1>
    </header>

    <section class="input-area">
      <input v-model="newActivity" placeholder="Tambah kegiatan baru..." @keyup.enter="addActivity" />
      <button @click="addActivity">Tambah</button>
    </section>

    <section class="filter-area">
      <label>Filter:</label>
      <select v-model="filterStatus">
        <option value="all">Semua</option>
        <option value="undone">Belum Selesai</option>
        <option value="done">Selesai</option>
      </select>
    </section>

    <section class="list-area">
      <transition-group name="fade" tag="ul">
        <li v-for="activity in filteredActivities" :key="activity.id" class="card">
          <label class="checkbox">
            <input type="checkbox" v-model="activity.done" />
            <span :class="{ done: activity.done }">{{ activity.name }}</span>
          </label>
          <button class="delete" @click="removeActivity(activity.id)">✕</button>
        </li>
      </transition-group>

      <p v-if="filteredActivities.length === 0" class="empty">Tidak ada kegiatan</p>
    </section>
  </div>
</template>

<style scoped>
/* Reset dan dasar */
* {
  box-sizing: border-box;
}
body {
  margin: 0;
  font-family: 'Segoe UI', sans-serif;
  background: #5e0606;
  color: #6a0606;
}

.wrapper {
  max-width: 700px;
  margin: 3rem auto;
  background: rgb(4, 42, 83);
  border-radius: 16px;
  padding: 2rem;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
}

/* Header */
header {
  text-align: center;
  margin-bottom: 2rem;
}
header h1 {
  font-size: 2rem;
  color: #b99fc5;
}
header span {
  color: #0d60bf;
  font-weight: 700;
}

/* Input */
.input-area {
  display: flex;
  gap: 0.5rem;
  margin-bottom: 1rem;
}
.input-area input {
  flex: 1;
  padding: 0.75rem 1rem;
  border: 1px solid #ccc;
  border-radius: 12px;
  font-size: 1rem;
}
.input-area button {
  padding: 0.75rem 1.2rem;
  background: #3c85e4;
  color: white;
  border: none;
  border-radius: 12px;
  cursor: pointer;
  transition: 0.3s;
}
.input-area button:hover {
  background: #379f6e;
}

/* Filter */
.filter-area {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  margin-bottom: 1.5rem;
}
.filter-area select {
  padding: 0.5rem 1rem;
  border-radius: 10px;
  border: 1px solid #84a336;
  background-color: #100101;
}

/* List area */
.list-area ul {
  list-style: none;
  padding: 0;
  margin: 0;
}
.card {
  background: #4e7ba8;
  border: 1px solid #e0e0e0;
  padding: 1rem;
  margin-bottom: 1rem;
  border-radius: 12px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  transition: all 0.3s ease;
}
.card:hover {
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
}

/* Checkbox */
.checkbox {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  flex: 1;
}
.checkbox input[type="checkbox"] {
  transform: scale(1.3);
  accent-color: #640a78;
}
.done {
  text-decoration: line-through;
  color: #999;
  transition: all 0.3s;
}

/* Delete */
.delete {
  background: none;
  border: none;
  color: #e74c3c;
  font-size: 1.5rem;
  cursor: pointer;
}
.delete:hover {
  color: #c0392b;
}

/* Empty state */
.empty {
  text-align: center;
  margin-top: 1rem;
  color: #aaa;
}

/* Animasi */
.fade-enter-active,
.fade-leave-active {
  transition: all 0.4s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  transform: translateY(10px);
}
</style>