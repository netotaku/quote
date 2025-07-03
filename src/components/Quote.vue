<template>
  <div class="quote">
    <p v-if="loading">Fetching wisdom...</p>
    <blockquote v-else>
      “{{ quote }}”
      <footer>— {{ author }}</footer>
    </blockquote>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const quote = ref('')
const author = ref('')
const loading = ref(true)

async function fetchQuote() {
  try {
    const res = await fetch('/.netlify/functions/quote')
    const data = await res.json()
    quote.value = data[0].q
    author.value = data[0].a
  } catch (error) {
    quote.value = 'Failed to fetch quote.'
    author.value = ''
  } finally {
    loading.value = false
  }
}

onMounted(fetchQuote)
</script>

<style scoped lang="scss">

</style>
