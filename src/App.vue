<script setup>
  import { provide, ref } from 'vue'
  import Quote from './components/Quote.vue'

  const lives = ref(4)

  provide("lives", lives)

</script>

<template>
  <Quote />
  <footer class="footer">
    <div class="lives">♡ {{ lives }}</div>
    <!-- <button class="button">Next quote</button> -->
  </footer>
</template>

<style scoped lang="scss">

  .footer{
    position: absolute;
    bottom: 5vw;
    left: 5vw;
    right: 5vw;
    font-size: .6em;
    display: flex;
    justify-content: space-between;
  }

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