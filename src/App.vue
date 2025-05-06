<script setup>
import { ref } from "vue";

// Daftar kegiatan (ref agar reaktif)
const activities = ref([
  { id: 1, name: "Meeting with team" },
  { id: 2, name: "Coffee with client" },
]);

// Input kegiatan baru
const newActivity = ref("");

// Fungsi untuk menambah kegiatan
const addActivity = () => {
  if (newActivity.value.trim() !== "") {
    activities.value.push({
      id: Date.now(),
      name: newActivity.value,
    });
    newActivity.value = "";
  }
};

// Fungsi untuk menghapus kegiatan
const removeActivity = (id) => {
  activities.value = activities.value.filter(activity => activity.id !== id);
};
</script>

<template>
  <div class="container">
    <!-- Judul -->
    <h1>Aplikasi Kegiatan Harian</h1>

    <!-- Form tambah kegiatan -->
    <div class="form">
      <input v-model="newActivity" placeholder="Masukkan kegiatan..." />
      <button @click="addActivity">Tambah</button>
    </div>

    <!-- Daftar kegiatan -->
    <h2>Daftar Kegiatan:</h2>
    <ul>
      <li v-for="activity in activities" :key="activity.id">
        {{ activity.name }}
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
  margin-bottom: 1.5em;
}

input {
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
