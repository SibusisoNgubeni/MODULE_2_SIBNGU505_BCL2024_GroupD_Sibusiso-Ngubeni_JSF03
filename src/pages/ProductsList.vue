<script setup>
import { ref, onMounted } from 'vue';


/**
 * Reactive reference to store the list of products.
 * @type {import('vue')}
 */
const products = ref([]);


/**
 * Fetches data from the Fake Store API and updates the `products` reference.
 * Handles errors by logging them to the console and resetting `products` to an empty array.
 * @async
 * @function
 * @throws {Error} Throws an error if the network response is not ok.
 */
const fetchData = async () => {
  try {
    const response = await fetch('https://fakestoreapi.com/products');
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }
    products.value = await response.json();
  } catch (err) {
    console.error(err);
    products.value = [];
  }
};

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

        <div v-if="products.length" class="product-list">
            <div v-for="product in products" :key="product.id" class="product-card">
                <img :src="product.image" :alt="product.title" class="product-image">
                <h2 class="product-title">{{ product.title }}</h2>
                <p class="product-price">${{ product.price.toFixed(2) }}</p>
                <p class="product-category">{{ product.category }}</p>
            </div>
        </div>
        <p v-else>Loading...</p>
    </div>

</template>

<style>
 .product-list{
     display: flex;
     flex-wrap: wrap;
     gap: 16px;
     justify-content: center;
    }

  .product-card{
     display: flex;
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