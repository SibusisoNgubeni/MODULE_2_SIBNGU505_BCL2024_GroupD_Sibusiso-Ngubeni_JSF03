<script setup>
import { ref, onMounted } from 'vue';
import Sorting from '../components/Sorting.vue';
import CategoryFilter from '../components/CategoryFilter.vue';

/**
 * Reactive reference to store the list of products.
 * @type {import('vue')}
 */
const products = ref([]);
const filteredProducts = ref([]);


const currentCategory = ref('');
const sortOption = ref('');

/**
 * Fetches data from the Fake Store API and updates the `products` reference.
 * Handles errors by logging them to the console and resetting `products` to an empty array.
 * @async
 * @function
 * @throws {Error} Throws an error if the network response is not ok.
 */
const fetchData = async (category = '') => {
    const url = category
          ? `https://fakestoreapi.com/products/category/${category}`
          : 'https://fakestoreapi.com/products';
  try {
    const response = await fetch(url);
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }
    products.value = await response.json();
    applyFilters();
  } catch (err) {
    console.error(err);
    products.value = [];
  }
};


function applyFilters() {
      const filtered = [...products.value];

      if (sortOption.value === 'asc') {
          filtered.sort((a, b) => a.price - b.price);
      } else if (sortOption.value === 'desc') {
          filtered.sort((a, b) => b.price - a.price);
      }

      filteredProducts.value = filtered;
  }

/**
 * Handles changes in the category selection.
 * Updates the currentCategory reference and fetches the filtered product data.
 * @param {string} category - The selected category.
 */

function handleCategoryChange(category) {
      currentCategory.value = category;
      fetchData(category);
  }

  function handleSortChange(sortOption) {
      sortOption.value = sortOption;
      applyFilters();
  }

  /**
   * Resets the category and sort options and fetches all products.
   */
  function handleReset() {
      currentCategory.value = '';
      sortOption.value = '';
      fetchData(); 
  }
/**
 * Lifecycle hook that is called when the component is mounted.
 * Triggers the `fetchData` function to load the product data.
 */

onMounted(() => {
  fetchData();
});

</script>

<template>
    <div>

        <Sorting @sortChange="handleSortChange" @reset="handleReset" />
        <CategoryFilter @categoryChange="handleCategoryChange" />

        <div v-if="products.length" class="product-list">
   
            <router-link :to="`/product/${product.id}`" v-for="product in filteredProducts" :key="product.id">
            <div v-for="product in products" :key="product.id" class="product-card">
                <img :src="product.image" :alt="product.title" class="product-image">
                <h2 class="product-title">{{ product.title }}</h2>
                <p class="product-price">${{ product.price.toFixed(2) }}</p>
                <p class="product-category">{{ product.category }}</p>
            </div>
            </router-link>
        </div>
        <p v-else>Loading...</p>
    </div>

</template>

<style scoped>
 .product-list{
     display: flex;
     flex-wrap: wrap;
     gap: 16px;
     justify-content: center;
    }

  .product-card{
     border: 1px solid black;
     border-radius: 10px;
     width: 350px;
     height: 500px;
     box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
     transition: transform 0.2s;
     display: flex;
     flex-direction: column;
     align-items: center; 
     text-align: center; 
     margin-top: 50px;
    }

  .product-title {
    padding: 10px;
    font-size: 1.2em;
    margin-bottom: 10px;
    height: 60px;
    }

  .product-image {
    width: 200px;
    height: 300px;
    object-fit: contain;
    margin-bottom: 10px;
    }

  .product-price {
    font-size: 1.2em;
    color: #333;
    margin-bottom: 10px;
    }

  .product-category {
    font-size: 20px;
    color: #777;
    }
</style>