<script setup>
import { ref, onMounted, watch } from 'vue';
import { useRoute, useRouter } from 'vue-router';
import Sorting from '../components/Sorting.vue';
import CategoryFilter from '../components/CategoryFilter.vue';

/**
 * Reactive reference to store the list of products and filter options.
 * @type {import('vue')}
 */
const products = ref([]);
const filteredProducts = ref([]);

/**
 * Reactive reference to store the current selected category and current selected sort option.
 * @type {Ref<string>}
 */
const currentCategory = ref('');
const sortOption = ref('');

const router = useRouter();
const route = useRoute();

/**
 * Fetches data from the Fake Store API and updates the `products` reference.
 * If a category is specified, it fetches products from that category.
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
 * Updates the URL query parameters to reflect the selected category.
 * @param {string} category - The selected category.
 */
function handleCategoryChange(category) {
      currentCategory.value = category;
      router.push({  query: {...route.query, category } });
      fetchData(category);
  }


/**
 * Handles changes in the sort option.
 * Updates the `sortOption` reference and applies the filters based on the selected sort option.
 * Updates the URL query parameters to reflect the selected sort option.
 * @param {string} option - The selected sort option.
 */
  function handleSortChange(option) {
      sortOption.value = option;
      router.push({ query: { ...route.query, sort: option } });
      applyFilters();
  }

  /**
   * Resets the category and sort options and fetches all products.
   * Clears the category and sort options in the URL query parameters.
   */
  function handleReset() {
      currentCategory.value = '';
      sortOption.value = '';
      router.push({ query: {} });
      fetchData(); 
  }
/**
 * Lifecycle hook that is called when the component is mounted.
 * Retrieves filter values from the URL and fetches the data accordingly.
 * Triggers the `fetchData` function to load the product data.
 */

onMounted(() => {
  const { category, sort } = route.query;
  if (category) {
    currentCategory.value = category;
  }
  if (sort) {
    sortOption.value = sort;
  }
  fetchData(currentCategory.value);
});

/**
 * Watches for changes in the URL query parameters.
 * Updates the `currentCategory` and `sortOption` references based on the new query parameters.
 * Calls `fetchData` with the updated category and sort option.
 * @param {Object} newQuery - The new query parameters from the URL.
 */
watch(() => route.query, (newQuery) => {
  if (newQuery.category) {
    currentCategory.value = newQuery.category;
  }
  if (newQuery.sort) {
    sortOption.value = newQuery.sort;
  }
  fetchData(currentCategory.value);
}, { immediate: true });

</script>

<template>
    <div>

        <Sorting @sortChange="handleSortChange" @reset="handleReset" />
        <CategoryFilter @categoryChange="handleCategoryChange" />

        <div v-if="filteredProducts.length" class="product-list">
   
            <router-link :to="`/product/${product.id}`" v-for="product in filteredProducts" :key="product.id" class="link">
            <div  :key="product.id" class="product-card">
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

.link{
  text-decoration: none;
}
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
    color: #000;
    padding: 10px;
    font-size: 1.2em;
    margin-bottom: 10px;
    height: 60px;
    }

  .product-image {
    width: 200px;
    height: 280px;
    object-fit: contain;
    margin: 10px;
    }

  .product-price {
    font-size: 1.2em;
    color: #333;
    margin: 5px;
    }

  .product-category {
    font-size: 20px;
    color: #777;
    }
</style>