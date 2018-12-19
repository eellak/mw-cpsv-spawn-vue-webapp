<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    <CPSVForm />
  </div>
</template>

<script>
// const axios = require('axios')
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'
import CPSVForm from '@/components/CpsvForm.vue'

export default {
  name: 'home',
  components: {
    // HelloWorld,
    CPSVForm
  }
}
</script>
