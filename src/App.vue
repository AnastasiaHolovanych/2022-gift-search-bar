<script setup>
import { ref, watch } from 'vue'

const searchTerm = ref('');
const data = ref([]);
const loading = ref(false);

const findProducts = async term => {
  loading.value = true;
  fetch(`https://dummyjson.com/products/search?q=${term}`)
    .then(res => res.json())
    .then(response => {
      data.value = response.products;
    })
    .catch((error) => {
      alert(error.message);
    })
    .finally(() => {
      loading.value = false;
    });
}

watch(() => searchTerm.value, (newTerm) => {
  if (newTerm) {
    setTimeout(() => {
      findProducts(newTerm);
    }, 300)
  } else {
    setTimeout(() => {
      data.value = [];
    }, 300)
  }
})
</script>

<template>
  <div class="w-full h-full flex flex-col gap-5 justify-center items-center">
    <h1 class="text-4xl font-bold">Gift Search Bar</h1>
    <input type="text" class="p-2 border-2 border-gray-dark" v-model="searchTerm" placeholder="Start typing..." />
    <p v-if="loading">Loading ...</p>
    <ul v-else class="list-disc">
      <li v-for='item in data'> {{`${item.title} - $${item.price}`}} </li>
    </ul>
  </div>
</template>

