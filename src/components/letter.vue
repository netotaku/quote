<template>
  <div v-if="!isPunctuation" class="letter">
    <input class="input" type="text" :value="char" />
    <label>10</label>
  </div>
  <div v-else class="letter">
    <span class="input">{{ char }}</span>
  </div>
</template>

<script setup>
import { computed } from 'vue'

const { char } = defineProps({
  char: String
})

const isPunctuation = computed(() => /^[^\w\s]$/.test(char))
</script>

<style scoped lang="scss">
    .letter{
        display: inline-block;
    }

    .input{
        display: block;
        appearance: none;
        font-family: inherit;
        font-size: inherit;
        color: inherit;
        width: .6em;
        height: 1.2em;
        border: 0;
        text-align: center;
        background: none;
        border: 1px orange;
        &:focus{
            background: none;
            outline: none;
            border-color: #f60;
        }
    }
    label{
        display: block;
        font-size: .3em;
        text-align: center;
        color: rgba(255,255,255, .5);
    }
</style>
