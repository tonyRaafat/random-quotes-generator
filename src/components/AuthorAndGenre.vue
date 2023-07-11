
<script setup>
    import { useRouter } from 'vue-router';
    const props = defineProps(['quoteAuthor','getAuthorQuotes'])
    const router = useRouter()
</script>

<template >
    <div class="wrapper clickable" @click="router.push(`/quotesFrom/${quoteAuthor}`)" >
        <h2  >{{ quoteAuthor }}</h2>
        <img style="padding:5px ;" src="../assets/arrow.svg" alt="">
    </div>

</template>

<style scoped>
 .wrapper{
    display: flex;
    justify-content: space-between;
    align-items: center;
 }
 .wrapper:hover{
  color: #e4e5ff;
  border: #ffffff solid 1px;
  background:#292929;
  transition: 0.3s;
}

 .clickable{
    cursor: pointer;
  }
  h2{
    padding: 15px 0 15px 6px;
    /* padding-left: 6px; */
  }
</style>