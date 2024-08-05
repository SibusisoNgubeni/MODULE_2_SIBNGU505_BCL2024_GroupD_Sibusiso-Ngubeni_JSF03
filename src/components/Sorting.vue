<script setup>
  import { ref, defineEmits } from 'vue';
    /**
   * Event dispatcher for emitting events to parent components.
   */
  const emit =  defineEmits();

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
   <button @click="handleReset">Reset</button>

</template>
