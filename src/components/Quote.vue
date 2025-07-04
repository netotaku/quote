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
  const letterCount = ref(0)

  provide('letterMap', letterMap) 
  provide('revealedLetters', revealedLetters)
  provide('letterCount', letterCount)

  /////  

  async function fetchQuote() {

    letterCount.value = 0

    const res = await fetch('/.netlify/functions/quote')
    const data = await res.json()
    
    quote.value = data[0].q
    author.value = data[0].a
    loading.value = false

    console.log(quote.value)

    generateLetterMap(quote.value)
    selectStarterLetters(quote.value, letterMap.value, revealedLetters)

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

  function sample(arr) {
    return arr[Math.floor(Math.random() * arr.length)]
  }

  function sampleMany(arr, count) {
    const copy = [...arr]
    const result = []
    while (result.length < count && copy.length) {
      const idx = Math.floor(Math.random() * copy.length)
      result.push(copy.splice(idx, 1)[0])
    }
    return result
  }

  function selectStarterLetters(text, map, revealedLetters) {
    const upper = text.toUpperCase()
    const vowels = ['A', 'E', 'I', 'O', 'U']
    const lettersOnly = upper.replace(/[^A-Z]/g, '')
    const uniqueLetters = [...new Set(lettersOnly)]

    const availableVowels = uniqueLetters.filter(l => vowels.includes(l))
    const availableConsonants = uniqueLetters.filter(l => !vowels.includes(l))

    const picks = []

    if (availableVowels.length) {
      picks.push(sample(availableVowels))
    }

    const consonantCount = Math.random() > 0.5 ? 2 : 1
    picks.push(...sampleMany(availableConsonants, consonantCount))

    let letterIndex = 0

    for (let i = 0; i < upper.length; i++) {
      const char = upper[i]

      if (/^[A-Z]$/.test(char)) {
        if (picks.includes(char)) {
          // Reveal only 1–2 instances of each letter
          if (!revealedLetters[letterIndex]) {
            revealedLetters[letterIndex] = char

            // Optional: limit per letter to 2 occurrences
            const count = Object.values(revealedLetters).filter(c => c === char).length
            if (count >= 2) picks.splice(picks.indexOf(char), 1)
          }
        }

        letterIndex++
      }
    }
    // console.log(revealedLetters)
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
