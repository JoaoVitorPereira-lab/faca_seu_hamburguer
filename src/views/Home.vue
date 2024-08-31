<template>
  <div>
    <Banner />
    <div class="main-container">
      <h1> Monte o seu Hambúrguer: </h1>
      <HamburguerForm />
    </div>
  </div>
</template>

<script>
import Banner from '../components/Banner'
import HamburguerForm from '../components/HamburguerForm'

export default {
  name: 'Home',
  components: {
    Banner,
    HamburguerForm
  }
}
</script>