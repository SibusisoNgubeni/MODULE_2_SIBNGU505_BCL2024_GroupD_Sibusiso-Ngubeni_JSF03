<script setup>
import { ref, onMounted } from 'vue';
import { useRoute } from 'vue-router';


const route = useRoute();
const product = ref(null);
const loading = ref(true);
const error = ref(null);



onMounted(async () => {
    ``
      const id = route.params.id; 

      try {
          const response = await fetch(`https://fakestoreapi.com/products/${id}`);
           if (!response.ok) {
               throw new Error('Failed to fetch data');
            }
          product.value = await response.json();
      } catch (err) {
            error.value = err.message || 'An error occurred';
         } finally {
             loading.value = false;
         }
    });

    function getRatingStars(rate) {
    const totalStars = 5;
    const filledStars = Math.floor(rate);
    const hasHalfStar = rate % 1 >= 0.5;
    return Array(totalStars).fill(false).map((_, index) => {
      if (index < filledStars) return 'filled';
      if (index === filledStars && hasHalfStar) return 'half';
      return 'empty';
    });
  }


</script>

<template>
  <div>
    <div v-if="loading">Loading...</div>

    <div v-else-if="error">
      <p>Error: {{ error }}</p>
    </div>

    <div v-else>
      <router-link to="/">
        <button class="return-btn">Back to products</button>
      </router-link>

      <div class="product-detail">
        <div class="product-card">
          <div class="product-image">
            <img :src="product.image" alt="Product Image" class="product-image"/>
          </div>
        </div>

        <div class="info-block">
          <div class="product-title">{{ product.title }}</div>
          <div class="product-category">{{ product.category }}</div>
          <div class="product-description">{{ product.description }}</div>

          <div class="rating">
            <span class="rating-rate">{{ product.rating.rate }}</span>
            <span v-for="(star, index) in getRatingStars(product.rating.rate)" :key="index">
              <svg
                :class="['rating-star', star]"
                xmlns="http://www.w3.org/2000/svg"
                fill="currentColor"
                viewBox="0 0 22 20"
                style="width: 20px; height: 20px;"
                aria-hidden="true"
              >
                <path d="M20.924 7.625a1.523 1.523 0 0 0-1.238-1.044l-5.051-.734-2.259-4.577a1.534 1.534 0 0 0-2.752 0L7.365 5.847l-5.051.734A1.535 1.535 0 0 0 1.463 9.2l3.656 3.563-.863 5.031a1.532 1.532 0 0 0 2.226 1.616L11 17.033l4.518 2.375a1.534 1.534 0 0 0 2.226-1.617l-.863-5.03L20.537 9.2a1.523 1.523 0 0 0 .387-1.575Z" />
              </svg>
            </span>
            <div class="rating-text">
              <span class="rating-count">{{ product.rating.count }} ratings</span>
            </div>
          </div>
        </div>

        <div class="buy-block">
          <div class="product-price">$ {{ product.price }}</div>
          <button>Add to cart</button>
        </div>
      </div>
    </div>
  </div>
</template>


<style scoped>

</style>