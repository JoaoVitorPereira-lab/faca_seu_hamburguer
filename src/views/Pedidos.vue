<template>
  <div class="main_container">
    <h1>Gerenciar Pedidos:</h1>
    <Dashboard />
  </div>
</template>

<script>
import Dashboard from '@/components/Dashboard.vue'

export default{
  name: "Pedidos",
  components:{
    Dashboard
  }
}
</script>