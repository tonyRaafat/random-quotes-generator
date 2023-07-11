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
    <img src="./assets/home.svg" class="clickable" @click="()=>router.push('/')">
    <div class="search-cont">
      <input  @keyup.enter="searchQuotes" type="text" placeholder="Select genre" id="genre-input">
      <button :disabled="isFetching" @click="searchQuotes" ><img src="../src/assets/searchIcon.svg" alt=""></button>
    </div>
    <button class="randomizer" :disabled="isFetching" @click="getQuote"><img src="../src/assets/random.svg" alt=""></button>
    
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
    padding: 8px 19px 5px 19px;
    color: #69748a;
  }
  input{
    all: unset;
    height: 100%;
    width: 100%;
    /* border: 1.2px solid #758cb8; */
    border: none ;
    border-radius: 6px;
    margin-right: 2px;
    padding: 4.5px;
    font-size: 1rem;
  }
  .search-cont{
    place-self: center;
    width: 60%;
    margin-left: auto;
    margin-right: auto;
    border: 1.2px solid #758cb8;
    border-radius: 6px;
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
    /* border: 1.2px solid #758cb8; */
    border: none;
    border-radius: 6px;
    padding: 6px;
    color: #69748a;
    cursor: pointer;
    font-size:1.5em;
    /* margin-left: auto; */
    /* outline:none; */
  }
</style>
