<script setup>
import HelloWorld from './components/HelloWorld.vue'
import Modal from './components/Modal.vue'
import Navbar from './components/Navbar.vue'
import Slider from './components/Slider.vue'
import TheWelcome from './components/TheWelcome.vue'

import { ref, onMounted } from 'vue'

// state variable
const posts = ref([]);
const loading = ref(true);
const error = ref(null);
const selectPost = ref(null);

// proses fetching data API
const fetchPosts = async () => {
  loading.value = true;
  error.value = null;

  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/posts');

    if (!response.ok) {
      throw new Error("GAGAL");
    }

    posts.value = await response.json();

  } catch (error) {
    console.error("Error fetch data");

  } finally {
    loading.value = false;
  }

};

// fungsi untuk lihat post detail
const viewDetails = (post) => {
  selectPost.value = post;
};

onMounted(fetchPosts);

</script>

<template>
  <Navbar />

  <Slider />

  <!-- Card -->
  <div class="card-container">
    <div v-for="post in posts" :key="post.id" class="card">
      <div class="card-title">{{ post.title }}</div>
      <p class="card-body">{{ post.body }}</p>
      <div class="card-footer">
        <span class="post-id">POST #{{ post.id }} </span>
        <button class="details-button">View Detail</button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.card-container {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 1.5rem;
}

.card {
  background-color: white;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  overflow: hidden;
  transition: transform 0.3s ease;
  display: flex;
  flex-direction: column;
}

.card:hover {
  transform: translateY(-5px);
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.15);
}

.card-title {
  font-size: 1.2rem;
  padding: 1rem 1rem 0.5rem;
  color: #333;
  text-transform: capitalize;
}

.card-body {
  font-size: 14px;
  color: #666;
  flex-grow: 1;
}

.card-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0.75rem 1rem;
  background-color: #f8f9fa;
  border-top: 1px solid #eee;
}

.post-id {
  font-size: 0.85rem;
  color: #888;
}

.details-button {
  background-color: #4398db;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  font-weight: bold;
  transition: bacground-color 0.2s;
}

.details-button:hover {
  background-color: #2980b9;
}
</style>
