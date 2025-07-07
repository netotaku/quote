<template>
  <div v-if="!isPunctuation" :class="[baseClass, incorrect ? 'incorrect' : '']">
    <input 
        class="input" 
        type="text" 
        v-model="displayValue"        
        :disabled="show"
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

    import { onMounted, computed, inject, ref } from 'vue'

    const displayValue  = ref(String);
    const defaultValue  = ref(String);
    const show          = ref(Boolean);
    const incorrect     = ref(false);
    const baseClass     = 'letter'
    
    const { char, i } = defineProps({ char: String, i: Number })

    const letterMap = inject('letterMap')
    const revealedLetters = inject('revealedLetters')
    const isPunctuation = computed(() => /^[^\w\s]$/.test(char))
    
    function init(){
        show.value = (revealedLetters[i] != undefined)
        displayValue.value = defaultValue.value = show.value ? char : '_'        
    }

    function handleFocus(){
        displayValue.value = ""
    }

    function handleBlur(){
        if( displayValue.value != defaultValue.value || displayValue.value == "" ){
            incorrect.value = false
            if( displayValue.value != "" ){
                if( displayValue.value.toUpperCase() == char.toUpperCase() ){
                    show.value = true
                    console.log("Correct", displayValue.value, char)
                } else {
                    incorrect.value = true
                    console.log("Incorrect", displayValue.value, char)
                }
            } else {
                displayValue.value = defaultValue.value
            }
        }   
    }

    //////////

    onMounted(init)
     
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

    .incorrect{
        color: red;
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
