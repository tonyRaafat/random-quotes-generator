<script setup>
import AuthorAndGenre from '../components/AuthorAndGenre.vue';
import Quote from '../components/Quote.vue'

const props = defineProps(['getQuote',"quoteText","quoteAuthor"])

</script>

<template >
  <div class="cont">
  <div class="quote">
    <Quote  :quoteText="quoteText" />
    <AuthorAndGenre  :quoteAuthor="quoteAuthor" />
  </div>
  </div>
</template>

<style scoped>
  .cont{
    /* height: 100%; */
    display: grid;
    place-items: center;
  }
  .quote{
    margin-top: 50px;
    padding: 19px;
    width: 60%;
    display: grid;
    align-items: center;
    justify-content: space-around;
}
</style>
