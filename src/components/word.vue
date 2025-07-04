<template>
  <span class="word">
    <Letter
      v-for="(char, index) in letters"
      :key="index"
      :char="char"
    />
  </span>
</template>

<script setup>
import Letter from './Letter.vue'
import { computed } from 'vue'

const props = defineProps({
  text: String
})

const letters = computed(() => props.text.split(''))
</script>

<style scoped lang="scss">
    .word {
        display: inline-flex;
        &:not(:last-of-type){
            margin-right: .4em;
        }
    }
</style>