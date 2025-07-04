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
import { ref, onMounted, computed } from 'vue'
import Word from './Word.vue'

const loading = ref(true)
const quote = ref('')
const author = ref('')

async function fetchQuote() {
  const res = await fetch('/.netlify/functions/quote')
  const data = await res.json()
  quote.value = data[0].q
  author.value = data[0].a
  loading.value = false
}

onMounted(fetchQuote)

const words = computed(() => {
  return quote.value.split(' ')
})
</script>

<style scoped lang="scss">
    cite{
      margin-top: .6em;  
      display: block;
    }
</style>
