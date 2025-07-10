<template>
  <div v-if="!isPunctuation" :class="[baseClass, incorrect ? 'incorrect' : '']">
    <input 
        autocapitalize="off"
        autocorrect="off"
        spellcheck="false"    
        class="input" 
        type="text" 
        maxlength="1" 
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

    const { char, i, fetchQuote } = defineProps({ 
        char: String, 
        i: Number,
        fetchQuote: Function 
    })

    const displayValue  = ref(String);
    const defaultValue  = ref(String);
    const show          = ref(Boolean);
    const incorrect     = ref(false);
    const baseClass     = 'letter'
    
    const letterMap         = inject('letterMap')
    const revealedLetters   = inject('revealedLetters')
    const lives             = inject('lives')
    const solvedCount       = inject('solvedCount')
    const letterCount       = inject('letterCount')
    const streak            = inject('streak')
    
    const isPunctuation = computed(() => /^[^\w\s]$/.test(char))
    
    function init(){
        show.value = (revealedLetters[i] != undefined)
        if(show.value){
            solvedCount.value++
        }        
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
                    // console.log("Correct", displayValue.value, char)
                    solvedCount.value++
                    // console.log(solvedCount.value, letterCount.value)
                    if(solvedCount.value == letterCount.value){
                        streak.value++
                        fetchQuote()
                    }
                } else {
                    incorrect.value = true
                    lives.value--
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
        text-transform: lowercase;
        // border: solid 1px orange;
        &:focus{
            background: none;
            outline: none;
           // border-color: rgba(255,255,255, .5);
        }
        &:disabled{
            color: white;
            opacity: 1; // important to override default greyed-out look
            -webkit-text-fill-color: white; // for Safari/iOS
        }
    }

    label{
        display: block;
        font-size: .3em;
        text-align: center;
        color: rgba(255,255,255, .5);
    }

</style>
