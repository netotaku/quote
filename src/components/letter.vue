<template>
  <div v-if="!isPunctuation" class="letter">
    <input 
        class="input" 
        type="text" 
        :value="show() ? char : '_'" 
        :disabled="show()"
        @focus="handleFocus"
        @blur="handleBlur"
    />
    <label>{{ letterMap[char.toUpperCase()] }}</label>
  </div>
  <div v-else class="letter">
    <span class="input">{{ char }}</span>
  </div>
</template>

<script setup>

    import { computed, inject } from 'vue'

    const { char, i } = defineProps({ char: String, i: Number })
    const letterMap = inject('letterMap')
    const revealedLetters = inject('revealedLetters')
    const isPunctuation = computed(() => /^[^\w\s]$/.test(char))

    function show(){
        return (revealedLetters[i] != undefined)
    }

    function handleFocus(){
        
    }

    function handleBlur(){
        
    }

    // console.log(i, char, show())       

</script>

<style scoped lang="scss">

    .letter{
        display: inline-block;
        &:hover{
            label{
                color: white;
            }
        }
    }

    .input{
        display: block;
        position: relative;
        border-radius: 3px;
        appearance: none;
        font-family: inherit;
        font-size: inherit;
        color: inherit;
        width: .6em;
        height: 1.2em;
        border: 0;
        text-align: center;
        background: none;
        // border: solid 1px orange;
        &:focus{
            background: none;
            outline: none;
           // border-color: rgba(255,255,255, .5);
        }
    }

    label{
        display: block;
        font-size: .3em;
        text-align: center;
        color: rgba(255,255,255, .5);
    }

</style>
