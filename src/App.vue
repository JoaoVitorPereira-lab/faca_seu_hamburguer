<template>
  <NavBar :logo="logo_src" :alt="app_name"/>
  <div>
    <router-view></router-view>
  </div>
  <Footer />
</template>

<script>
import NavBar from './assets/NavBar.vue';
import Footer from './assets/Footer.vue';

export default{
  components: {
    NavBar,
    Footer
  },
  data(){
    return{
      logo_src: "/img/logo.png",
      app_name: "Make Your Burger"
    }
  }
}
</script>

<style>
* {
  font-family: Arial, Helvetica, sans-serif;
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

.main_container{
  margin: 50px;
  min-height: 26rem;
}

h1{
  text-align: center;
  font-size: 2.5rem;
  margin-bottom: 30px;
  color: #222;
}
</style>
