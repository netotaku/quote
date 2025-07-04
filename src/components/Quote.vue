<template>
  <div class="quote">
    <p v-if="loading">Fetching wisdom...</p>
    <blockquote v-else class="quote">
      <Word
        v-for="(word, index) in words"
        :key="index"
        :text="word"
      />
      <cite>— {{ author }}</cite>
    </blockquote>
  </div>
</template>

<script setup>

  import { ref, onMounted, computed, provide } from 'vue'
  import Word from './Word.vue'

  const loading = ref(true)
  const quote = ref('')
  const author = ref('')
  const letterMap = ref({}) 
  const revealedLetters = ref({}) 
  const words = computed(() => { return quote.value.split(' ') })

  provide('letterMap', letterMap) 
  provide('revealedLetters', revealedLetters)

  /////  

  async function fetchQuote() {

    const res = await fetch('/.netlify/functions/quote')
    const data = await res.json()
    
    quote.value = data[0].q
    author.value = data[0].a
    loading.value = false

    generateLetterMap(quote.value)

  }

  ///// 

  function generateLetterMap(text) {
    // 1. Extract uppercase letters only
    const lettersOnly = text.toUpperCase().replace(/[^A-Z]/g, '')

    // 2. Get unique letters
    const unique = [...new Set(lettersOnly)]

    // 3. Shuffle
    const shuffled = unique.sort(() => Math.random() - 0.5)

    // 4. Map letters to 2-digit codes
    const map = {}
    shuffled.forEach((letter, index) => {
      map[letter] = (index + 1).toString().padStart(2, '0')
    })

    letterMap.value = map

  }

  /////

  onMounted(fetchQuote)

</script>

<style scoped lang="scss">
    cite{
      margin-top: .6em;  
      display: block;
    }
</style>
