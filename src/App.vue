<script setup>
import { RouterLink, RouterView,useRouter } from 'vue-router'
import {ref,onBeforeMount} from 'vue'
const router = useRouter()
let quote = ref()
let theAuthor = ref()
let number ;
let isFetching = ref(false);
let inputValue ;
let isNotFound = false
function getRandom(myNumber){
  let randomNum = Math.floor(Math.random()* 10)
  while(randomNum == myNumber){
     randomNum = Math.floor(Math.random()* 10)
  }
  number = randomNum
}
async function getResponse(author,genre) {
    let response = await fetch("https://quote-garden.onrender.com/api/v3/quotes?author="+author+"&genre="+genre)
    const jsonResponse = await response.json();
    return jsonResponse
}

  function searchQuotes(){
    inputValue = document.getElementById("genre-input").value
    getQuote('',inputValue);
  }

  async function getQuote(author='',genre='') {
    router.push("/")
    try{
      if(isFetching.value===false){
      isFetching.value = true;
      let data = await getResponse(author,genre)
      isFetching.value = false;
      console.log(data);
      getRandom(number)
      quote.value = data.data[number]["quoteText"]
      theAuthor.value = data.data[number]["quoteAuthor"]
      isNotFound = false
      
    }
    }
    catch (error) {
        console.log('Error:', error);
        isNotFound = true
        quote.value = ''
        theAuthor.value = ''
    }
 
  }
  onBeforeMount(() => {
    getQuote()
  })

</script>

<template >
  <header> 
    <h2 class="clickable" @click="()=>router.push('/')">Quote Generator</h2> 
    <div class="search-cont">
      <input  @keyup.enter="searchQuotes" type="text" placeholder="Select genre" id="genre-input">
      <button :disabled="isFetching" @click="searchQuotes" >search</button>
    </div>
    <button id="randomizer" :disabled="isFetching" @click="getQuote">random</button>
  </header>
  <div class="not-found" v-if="isNotFound" >
      No quotes found for this genre! 🤷‍♂️
  </div>
  <RouterView v-if="quote&&theAuthor" :getResponse="getResponse" :quoteText="quote" :quoteAuthor="theAuthor" />
</template>

<style scoped>
 header{
    height: 3rem;
    display: flex;
    padding: 5px 19px 5px 19px;
    color: #69748a;
  }
  input{
    height: 100%;
    width: 100%;
    border: 1.2px solid #758cb8;
    border-radius: 6px;
    margin-right: 2px;
    padding: 4.5px;
    font-size: 1.5em;
  }
  .search-cont{
    width: 60%;
    margin-left: auto;
    margin-right: auto;
    display: flex;
    align-items: center;
  }
  .not-found{
    font-size: 1.7rem;
    height: 80%;
    display: grid;
    place-items: center;
  }
  button{
    height: 100%;
    background:#fff;
    border: 1.2px solid #758cb8;
    border-radius: 6px;
    color: #69748a;
    cursor: pointer;
    font-size:1.5em;
    margin-left: auto;
    outline:none;
  }
</style>
