<script setup>
const props = defineProps(['quoteText'])

</script>

<template>
    <div class="quote-cont">
      <h1>{{ quoteText }}</h1>
    </div>

  
</template>

<style scoped>
  
  h1{
    padding: 2rem;
    font-weight: normal;
    border-left: 5px solid aqua;
    background-image: linear-gradient(to right ,rgb(188, 217, 217),rgb(196, 228, 228));
  }
  /* h2:hover{
    
  } */
 
</style>
