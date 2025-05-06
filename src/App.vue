<script setup>
import { ref, computed } from "vue";

// Daftar kegiatan (dengan properti 'done')
const activities = ref([
  { id: 1, name: "Meeting with team", done: false },
  { id: 2, name: "Coffee with client", done: false },
]);

// Input kegiatan baru
const newActivity = ref("");

// Filter aktif: true = tampilkan hanya yang belum selesai
const showOnlyUndone = ref(false);

// Fungsi menambah kegiatan
const addActivity = () => {
  if (newActivity.value.trim() !== "") {
    activities.value.push({
      id: Date.now(),
      name: newActivity.value,
      done: false,
    });
    newActivity.value = "";
  }
};

// Fungsi menghapus kegiatan
const removeActivity = (id) => {
  activities.value = activities.value.filter(activity => activity.id !== id);
};

// Data kegiatan yang akan ditampilkan (tergantung filter)
const filteredActivities = computed(() => {
  return showOnlyUndone.value
    ? activities.value.filter(activity => !activity.done)
    : activities.value;
});
</script>

<template>
  <div class="container">
    <h1>Aplikasi Kegiatan Harian</h1>

    <!-- Form input -->
    <div class="form">
      <input v-model="newActivity" placeholder="Masukkan kegiatan..." />
      <button @click="addActivity">Tambah</button>
    </div>

    <!-- Filter -->
    <div class="filter">
      <label>
        <input type="checkbox" v-model="showOnlyUndone" />
        Tampilkan hanya yang belum selesai
      </label>
    </div>

    <!-- Daftar kegiatan -->
    <h2>Daftar Kegiatan:</h2>
    <ul>
      <li v-for="activity in filteredActivities" :key="activity.id">
        <label class="checkbox-label">
          <input type="checkbox" v-model="activity.done" />
          <span :class="{ done: activity.done }">{{ activity.name }}</span>
        </label>
        <button class="delete" @click="removeActivity(activity.id)">❌</button>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.container {
  max-width: 500px;
  margin: 2em auto;
  font-family: Arial, sans-serif;
  text-align: center;
}

h1 {
  color: #42b883;
}

.form {
  margin-bottom: 1em;
}

input[type="text"] {
  padding: 0.5em;
  width: 60%;
  margin-right: 0.5em;
  border: 1px solid #ccc;
  border-radius: 4px;
}

button {
  padding: 0.5em 1em;
  background-color: #42b883;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.filter {
  margin-bottom: 1.5em;
  text-align: left;
  font-size: 0.9em;
  color: #333;
}

ul {
  list-style: none;
  padding: 0;
  text-align: left;
}

li {
  background: #0a0505;
  color: white;
  margin: 0.5em 0;
  padding: 0.75em;
  border-radius: 5px;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.checkbox-label {
  display: flex;
  align-items: center;
  gap: 0.5em;
  flex: 1;
}

.done {
  text-decoration: line-through;
  color: #aaa;
  transition: all 0.3s ease;
  opacity: 0.7;
}

.delete {
  background-color: #ff4d4d;
  padding: 0.3em 0.6em;
  border: none;
  border-radius: 4px;
  color: white;
  font-size: 0.9em;
  cursor: pointer;
}
</style>