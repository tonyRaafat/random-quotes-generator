<script  setup>
import { useRoute } from 'vue-router';
import Quote from '../components/Quote.vue'
import { ref ,onMounted} from 'vue';

  const {getResponse} = defineProps(['getResponse'])
  const route = useRoute();
  let data = ref() ;
  let {author} = route.params 
    getResponse(author,'').then((response)=>{
    return response
  }).then(resolvedData=>{
    data.value = resolvedData.data
  })
  
  
</script>

<template>
  <div class="cont">
    <h1>{{ author }} </h1>
    <div class="quote-cont" v-for="quote in data" :key="data._id">
      <Quote :quoteText="quote.quoteText"/>

    </div>

  </div>
</template>

<style scoped>
  .quote-cont{
    padding-top: 1rem;
    padding-left: 1rem;
    padding-right: 1rem;
  }
  .cont{
    width: 100%;
    display: grid;
    /* justify-content: start; */
    align-items: start;
    padding-top: 10px;
    padding-bottom: 10px;
  }
  h1{
    width: 100%;
   text-align: center;
  }
</style>
