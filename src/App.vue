<script setup>
  import { provide, ref } from 'vue'
  import Quote from './components/Quote.vue'

  const lives = ref(4)
  const streak = ref(0)

  provide("lives", lives)
  provide("streak", streak)

</script>

<template>
  <Quote />
</template>

<style scoped lang="scss">



  .button{
    appearance: none;
    background: #f60;
    border: 0;
    font-family: inherit;
    font-size: .3em;
    cursor: pointer;
    padding: 12px 20px;
    color: #fff;
    &:hover{
      background: #f90;
    }
  }

</style>