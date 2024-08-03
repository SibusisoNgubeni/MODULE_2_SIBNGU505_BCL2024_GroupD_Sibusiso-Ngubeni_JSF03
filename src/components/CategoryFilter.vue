<script setup>
import { ref, onMounted, defineEmits } from 'vue';

/**
 * Emits events to parent component.
 * @typedef {Object} Emits
 */
const emit = defineEmits(['categoryChange']);


/**
 * Reactive reference to store the list of categories.
 * @type {import('vue')}
 */
const categories = ref([]);

/**
 * Reactive reference to store the currently selected category.
 * @type {import('vue')}
 */
const selectedCategory = ref('');

onMounted(async () => {
  try {
    const response = await fetch('https://fakestoreapi.com/products');
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }
    const products = await response.json();
    categories.value = [...new Set(products.map(product => product.category))];
  } catch (error) {
    console.error('Failed to fetch products:', error);
  }
});


/**
 * Handles changes in the category selection dropdown.
 * Updates the selectedCategory reference and emits the categoryChange event
   with the selected category.
 * @param {Event} event - The change event from the category selection dropdown.
 */
function handleChange(event) {
  const category = event.target.value;
  selectedCategory.value = category;
  emit('categoryChange', category); 
}
</script>

<template>
   <span class="category-filter">

    <p>Categories</p>
  <select class="selector" @change="handleChange">
    <option value="">All Categories</option>
    <option v-for="category in categories" :key="category" :value="category">
      {{ category }}
    </option>
  </select>
   </span>
</template>

<style scoped>

.category-filter{
    height: 40px;
    width: 150px;
    margin-left: 50px;
    
    border-radius: 5px;
}

.selector{
    width: 150px;
    height: 100%;
    padding: 10px;
    background-color: rgb(149, 239, 234);
    
}
</style>