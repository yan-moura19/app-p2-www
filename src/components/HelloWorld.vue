<script setup>
import { onMounted, ref } from 'vue'
import  axios  from 'axios'

defineProps({
  
})
onMounted(()=>{
  getAtividades()
})
const getAtividades = ()=>{
  axios.get('https://localhost:7037/atividade')
        .then(response => {
          items.value = response.data;
          console.log(response.data)
        })
        .catch(error => {
          console.error(error);
        });

}

const url = ref('')
const count = ref(0)
const rota = ref("home")
const nome = ref("")
const editar = ref({})
const items = ref([
  {dataCadastro: '20/12/2022', nome: "Atividade 1", status: 'Finalizada', dataFinalizacao: "27/05/2023"},
  {dataCadastro: '21/12/2022', nome: "Atividade 2", status: 'Em andamento', dataFinalizacao: ""},
  {dataCadastro: '23/12/2022', nome: "Atividade 3", status: 'Em andamento', dataFinalizacao: ""},
  {dataCadastro: '24/12/2022', nome: "Atividade 4", status: 'Em andamento', dataFinalizacao: ""},
  {dataCadastro: '25/12/2022', nome: "Atividade 5", status: 'Em andamento', dataFinalizacao: ""},
  
  ])
</script>

<template>
  
  <VContainer>
    <v-card>
    <v-layout >
      <!-- <v-system-bar color="deep-purple darken-3"></v-system-bar> -->

      <v-app-bar
        color="blue"
        prominent
      >
        
        <v-btn variant="text" icon="mdi-home" @click="rota = 'home'"></v-btn>

        <v-toolbar-title>Atividades</v-toolbar-title>
        <v-spacer></v-spacer>
        <v-btn variant="text" icon="mdi-plus" @click="rota = 'novo'"></v-btn>
 
      </v-app-bar>
      <br><br>
           <v-main >
            <div v-if="rota === 'editar'">
              <v-form v-model="formEditar">
                  <v-container>
                    <v-row>
                      <v-col
                        cols="12"
                        md="4"
                      >
                        <v-text-field
                          v-model="editar.nome"
                          
                          
                          :counter="100"
                          label="Nome da atividade"
                          required
                        ></v-text-field>
                      </v-col>   
                    </v-row>
                    <v-btn type="button" @click="rota = 'home'" block class="mt-2">Submit</v-btn>
                  </v-container>
                </v-form>
              </div>
        <div v-if="rota === 'novo'">

          <v-form v-model="valid">
              <v-container>
                <v-row>
                  <v-col
                    cols="12"
                    md="4"
                  >
                    <v-text-field
                      v-model="nome"
                      :rules="nameRules"
                      :counter="10"
                      label="Nome da atividade"
                      required
                    ></v-text-field>
                  </v-col>   
                </v-row>
                <v-btn type="button" @click="rota = 'home'" block class="mt-2">Submit</v-btn>
              </v-container>
            </v-form>
        </div> 
        <div v-if="rota === 'home'" block>
          <div block v-for="(item, index) in items" :key="index">
            <v-card block >
              <template v-slot:title >
                <v-row class="mt-0  mb-0 pl-4 mr-2 py-2" >
                  <span>{{ item.nome }}</span>
                <v-spacer></v-spacer>
                 <v-btn  @click="rota = 'editar';editar = item" type="button" icon="mdi-pencil"  width="40" ></v-btn></v-row>
                
              </template>
              <template v-slot:subtitle>
                Data de cadastro: {{ item.dataCadastro }}
              </template>
              <template v-slot:text>
                <p v-if="item.dataFinalizacao !== ''">Finalizada dia: {{ item.dataFinalizacao }} <v-icon color="blue">mdi-check-all</v-icon></p>
                <p v-else> Não finalizada <v-icon >mdi-check</v-icon></p>
              </template>
              <v-card-actions  v-if="item.status != 'Finalizada'">
                
                <v-btn 
                v-if="item.status !== 'Finalizada'" 
                @click="items[index].status = 'Finalizada';items[index].dataFinalizacao = '27/05/2023'" 
                type="button"   
                class="mt-2">Finalizar
              </v-btn>
              
              </v-card-actions>
              
              
            </v-card>
            <br>
          </div>
        </div>
      </v-main>
    </v-layout>
  </v-card>
    
   
  

 
  
  </VContainer>
  
  
 
</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
