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

    <footer class="footer">
      <div class="lives">♡ {{ lives }} / Streak {{streak }}</div>
      <a @click.prevent="next" href="">Next quote</a>
    </footer>
  
  </div>
</template>

<script setup>

  import { ref, onMounted, computed, provide, inject } from 'vue'
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
  
  const lives = inject('lives')
  const streak = inject('streak')

  /////  

  async function fetchQuote() {

    loading.value = true
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

  function next(){
    lives.value--
    fetchQuote()
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

  const revealSettings = {
    vowels: 2,
    consonants: 4,
    instancesPerLetter: 3
  }

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

  function selectStarterLetters(text, map, revealedLetters, settings = revealSettings) {
    
    const upper = text.toUpperCase()
    const vowels = ['A', 'E', 'I', 'O', 'U']
    const lettersOnly = upper.replace(/[^A-Z]/g, '')
    const uniqueLetters = [...new Set(lettersOnly)]

    const availableVowels = uniqueLetters.filter(l => vowels.includes(l))
    const availableConsonants = uniqueLetters.filter(l => !vowels.includes(l))

    const picks = []

    // ✅ pick up to `settings.vowels` vowels
    picks.push(...sampleMany(availableVowels, settings.vowels))

    // ✅ pick up to `settings.consonants` consonants
    const consonantCount = Math.min(settings.consonants, availableConsonants.length)
    picks.push(...sampleMany(availableConsonants, consonantCount))

    const instancesPerLetter = {}

    let letterIndex = 0

    for (let i = 0; i < upper.length; i++) {
      const char = upper[i]

      if (/^[A-Z]$/.test(char)) {
        if (picks.includes(char)) {
          if (!instancesPerLetter[char]) {
            instancesPerLetter[char] = 0
          }

          if (instancesPerLetter[char] < settings.instancesPerLetter) {
            revealedLetters[letterIndex] = char
            instancesPerLetter[char]++
          }
        }

        letterIndex++
      }
    }
  }

  /////

  onMounted(fetchQuote)

</script>

<style scoped lang="scss">
    cite{
      margin-top: .6em;  
      display: block;
    }

    .footer{
      padding: 5vw;
      position: fixed;
      bottom: 0;
      left: 0;
      right: 0;
      font-size: .4em;
      display: flex;
      justify-content: space-between;
      background: rgba(0,0,0,.1)
    }
</style>
