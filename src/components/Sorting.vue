<script setup>
  import { ref, defineEmits } from 'vue';
    /**
   * Event dispatcher for emitting events to parent components.
   */
  const emit =  defineEmits(['sortChange', 'reset']);

    /** 
   * The currently selected sort option.
   * @type {string}
   */
  const sortOption = ref('');


   /**
   * Handles changes in the sort option dropdown selection.
   * Dispatches a 'sortChange' event with the selected sort option.
   */

   
  function handleSortChange(event) {
      sortOption.value = event.target.value;

      //changed sortOption.value to e.target.value
      emit('sortChange', event.target.value);
  }

  function handleReset() {
      emit('reset');
  }
</script>


<template>
<select class="sort" @change="handleSortChange">
    <option value="">Sort by Price</option>
    <option value="asc">Price: Low to High</option>
    <option value="desc">Price: High to Low</option>
</select>
   <button class="reset-btn" @click="handleReset">Reset</button>

</template>

<style scoped>
  .sort {
    height: 40px;
    width: 150px;
    margin-left: auto; /* Centering element horizontally */
    margin-right: auto; /* Centering element horizontally */
    margin-top: 20px;
    background-color: rgb(149, 239, 234);
    border-radius: 5px;
    display: flex;
    align-items: center; /* Center content vertically */
    justify-content: center; /* Center content horizontally */
  }

  /* Media Queries for Responsiveness */

  @media (max-width: 1200px) {
    .sort {
      width: 90%; /* Adjust width for medium screens */
      margin-left: 0;
    
    }
    .reset-btn{
        margin: 5px;
        margin-left: 0px;
        border-radius: 5px;
    }
  }

  @media (max-width: 992px) {
    .sort {
      width: 90%; /* Adjust width for smaller screens */
      margin-left: 0;
    }
    .reset-btn{
        margin: 5px;
        margin-left: 0px;
        border-radius: 5px;
    }
  }

  @media (max-width: 768px) {
    .sort {
      width: 90%; /* Adjust width for very small screens */
      margin-left: 5px; /* Adjust margin to fit smaller screens */
      margin-right: 10px;
    }
    .reset-btn{
        margin: 5px;
        margin-left: 0px;
        border-radius: 5px;
    }
  }

</style>
