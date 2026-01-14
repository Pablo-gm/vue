<script setup>
  import {ref} from 'vue'

  import Header from '@/components/Header.vue';
  import Main from '@/components/Main.vue';
  import Footer from '@/components/Footer.vue';

</script>

<template>
  <div class="container text-center my-5">
    <div class="row justify-content-center">
      <div class="col-12 col-md-4">
        <Header/>
        <Main/>
        <Footer/>
      </div>
    </div>
  </div>
</template>

<style scoped>
  p {
    color:cornflowerblue;
  }
</style>
