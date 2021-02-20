<template>
 <div>
   <form @submit="responder" method="post" >
     <template v-for="pergunta in perguntas" >
       <div :key="pergunta.id" v-if="!(pergunta.resposta_dependente.split(';').includes(respostas[pergunta.pergunta_dependente]))">
         <b-jumbotron style="width: 70%;margin:0 auto;text-align: center;padding:0px">
          <div style="margin:0 auto;text-align: center;padding:12px">
            <b-card bg-variant="dark" text-variant="white" title="">
              <b-card-text style="font-family: sans-serif;font_size:8">
                <h4>{{pergunta.perguntas}}</h4>
              </b-card-text>
            </b-card>
          </div>
          <div v-if="!!pergunta.opcoes">
            <div :key=index v-for="(item,index) in pergunta.opcoes.split(';')">
              <input :type="pergunta.id==18?'checkbox':'radio'" :id="'pergunta_'+pergunta.id+index" :name ="'pergunta_'+pergunta.id" :value =item v-model='respostas[pergunta.id]' >
              <label :for="'pergunta_'+pergunta.id+index">{{item}}</label>
            </div>
          </div>
      <input style ="width: 90%;" :key="Math.random()" :id="'pergunta_'+pergunta.id" :name ="'pergunta_'+pergunta.id" v-else type="text" autocomplete="off" v-model.lazy="respostas[pergunta.id]" >
      </b-jumbotron>
      </div>
     </template>
      <!-- {{respostas}} -->
     <b-button class="b-button" variant="success" btn-success v-on:click="responder">Enviar respostas</b-button>
   </form>

 </div>
</template>

<script>
import Vue from 'vue'
import axios from 'axios'
import VueAxios from 'vue-axios'
Vue.use(VueAxios,axios)

export default {
  name: 'Perguntas',
  mounted(){
    this.iniciar()
  },
  data(){
    return{
      pergunta_position:1,
      perguntas:[0],
      respostas: {18:[]},
    }
  },
  methods:{
    iniciar(){
      axios.get("https://formularioresponsivel.herokuapp.com/perguntas").then((response) => 
      {this.perguntas  = response.data})
    },
    proxima_pergunta(){
      if(this.pergunta_position<=18){
          this.pergunta_position+=1
      }
    }, 
    pergunta_anterior(){
      if(this.pergunta_position>1){
          this.pergunta_position-=1
      }
    },    
    responder(){
        this.axios.post("https://formularioresponsivel.herokuapp.com/respostas", {"respostas":JSON.stringify(this.respostas)})
        alert("Salvo com sucesso!")
    }
  }
  
}
</script>

<style scoped>
  label{
    padding-left: 5px;
  }
  input[type=text]:focus {
  border: 3px solid #555;
  }
  .b-button{
    margin:50px;
    padding:20px;
    width:70%;
    font-weight: bold;
    
  }
</style>
