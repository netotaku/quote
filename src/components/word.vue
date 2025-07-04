<template>
  <span class="word">
    <Letter
      v-for="(letter, index) in lettersWithIndices"
      :key="index"
      :i="letter.index"
      :char="letter.char"
    />
  </span>
</template>

<script setup>
import { ref, onBeforeMount, inject } from 'vue'
import Letter from './Letter.vue'

const letterCount = inject('letterCount') // Should be a ref from parent

// debug //////
// if (!letterCount || typeof letterCount.value !== 'number') {
//   throw new Error('letterCount must be injected as a ref and initialised as a number.')
// }
///////////////

const props = defineProps({
  text: String
})

const lettersWithIndices = ref([])

onBeforeMount(() => {
  lettersWithIndices.value = []

  for (const char of props.text) {
    const upper = char.toUpperCase()
    const isLetter = /^[A-Z]$/.test(upper)

    // Only increment for valid letters
    const index = isLetter ? letterCount.value++ : null

    // console.log(index, upper)

    lettersWithIndices.value.push({
      char,
      index
    })
  }
})
</script>

<style scoped lang="scss">
    .word {
        display: inline-flex;
        &:not(:last-of-type){
            margin-right: .4em;
        }
    }
</style>