<template>
   <Message :msg="msg" v-show="msg" />
   <div>
      <form id="hamburguer-form" method="POST" @submit.prevent="createHamburguer">
         <div class="input-container">
            <label for="nome">Nome do cliente:</label>
            <input type="text" id="nome" name="nome" v-model="nome" placeholder="Digite o seu nome" @change="createHamburguer">
         </div>
         <div class="input-container">
            <label for="pao">Escolha o pão:</label>
            <select name="pao" id="pao" v-model="pao">
               <option value="">Selecione o seu pão</option>
               <option v-for="pao in paes" :key="pao.id" :value="pao.tipo">
                  {{ pao.tipo }}
               </option>
            </select>
         </div>
         <div class="input-container">
            <label for="carne">Escolha a carne do seu Hambúrguer:</label>
            <select name="carne" id="carne" v-model="carne">
               <option value="">Selecione o tipo de carne</option>
               <option v-for="carne in carnes" :key="carne.id" :value="carne.tipo">
                  {{ carne.tipo }}
               </option>
            </select>
         </div>
         <div id="opcionais-container" class="input-container">
            <label id="opcionais-title" for="opcionais">Selecione os opcionais:</label>
            <div class="checkbox-container" v-for="opcional in opcionaisdata" :key="opcional.id">
               <input type="checkbox" name="opcionais" v-model="opcionais" :value="opcional.tipo">
               <span>{{ opcional.tipo }}</span>
            </div>
         </div>
         <div class="input-container">
            <input class="submit-btn" type="submit" value="Criar meu Hambúrguer!">
         </div>
      </form>
   </div>
</template>

<script>
import { toast } from "vue3-toastify";
import "vue3-toastify/dist/index.css";

import Message from './Message.vue'

export default {
   name: "HamburguerForm",
   data() {
      return {
         paes: null,
         carnes: null,
         opcionaisdata: null,
         nome: null,
         pao: null,
         carne: null,
         opcionais: [],
         status: "Solicitado",
         msg: null,
      }
   },
   methods: {
      async getIngredientes() {
         const req = await fetch("http://localhost:3000/ingredientes");
         const data = await req.json();

         this.paes = data.paes
         this.carnes = data.carnes
         this.opcionaisdata = data.opcionais
      },
      async createHamburguer(e) {
         if (!this.nome || this.nome.trim() === '') {
            return toast("Digite seu nome!", {
               "theme": "colored",
               "type": "error",
               "position": "top-left"
            });
         }
         if (!this.pao) {
            return toast("Escolha o pão!", {
               "theme": "colored",
               "type": "error",
               "position": "top-left"
            });
         }
         if (!this.carne) {
            return toast("Escolha a carne!", {
               "theme": "colored",
               "type": "error",
               "position": "top-left"
            });
         }
         
         e.preventDefault();

         const data = {
            nome: this.nome,
            carne: this.carne,
            pao: this.pao,
            opcionais: Array.from(this.opcionais),
            status: "Solicitado"
         }

         const dataJson = JSON.stringify(data)


         const req = await fetch("http://localhost:3000/hamburguer", {
            method: "POST",
            headers: { "Content-Type": "application/json" },
            body: dataJson
         });

         const res = await req.json()

         this.msg = `Pedido ${res.nome} burger realizado com sucesso!`

         toast("Pedido realizado com sucesso!", {
            "theme": "colored",
            "type": "success",
            "position": "top-left"
         })

         // clear message
         setTimeout(() => this.msg = "", 3000)

         // limpar campos
         this.nome = ""
         this.carne = ""
         this.pao = ""
         this.opcionais = []

      }
   },
   mounted() {
      this.getIngredientes()
   },
   components: {
      Message
   }
}
</script>

<style scoped>
#hamburguer-form {
   max-width: 400px;
   margin: auto;
}

.input-container {
   display: flex;
   flex-direction: column;
   margin-bottom: 20px;
}

label {
   font-weight: bold;
   margin-bottom: 15px;
   color: #222;
   ;
   padding: 5px 10px;
   border-left: 4px solid #fcba03;
}

input,
select {
   padding: 5px 10px;
   width: 300px;
}

#opcionais-container {
   flex-direction: row;
   flex-wrap: wrap;
}

#opcionais-title {
   width: 100%;
   margin-bottom: 1.5rem;
}

.checkbox-container {
   display: flex;
   align-items: flex-start;
   width: 50%;
   margin-bottom: 20px;
}

.checkbox-container span,
.checkbox-container input {
   width: auto;
}

.checkbox-container span {
   margin-left: 6px;
   margin-top: -5px;
   font-weight: bold;
}

.submit-btn {
   background-color: #222;
   color: #fcba03;
   font-weight: bold;
   border: 2px solid #222;
   padding: 10px;
   font-size: 16px;
   margin: 0;
   cursor: pointer;
   transition: .5s;
}

.submit-btn:hover {
   background-color: transparent;
   color: #222;
}
</style>