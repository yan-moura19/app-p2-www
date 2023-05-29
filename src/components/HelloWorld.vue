<script setup>
import { onMounted, ref } from 'vue'
import  axios  from 'axios'

defineProps({
  
})
onMounted(()=>{
  getAtividades()
})
const getAtividades = ()=>{
  
  axios.get(url.value)
        .then(response => {
          items.value = response.data;
          
        })
        .catch(error => {
          console.error(error);
        });
}
const createAtividade = () => {
      axios.post(url.value, { nome: nome.value})
        .then(response => {
          
          nome.value = '';
          getAtividades();
          rota.value = 'home'
        })
        .catch(error => {
          console.error(error);
        });
    };
const putAtividade= async (atividade) => {
  let idAtividade = atividade.id;
  let nomeEditar = atividade.nome
  
  await axios.put(url.value + "/" + idAtividade, {nome: nomeEditar})
              .then(()=>{
                alert("editado");
                 rota.value = 'home';
                 getAtividades();
                 editar.value = {}
                }).catch('erro ao editar')
  
}
async function finalizar(atividade){
  let idAtividade = atividade.id;
  
  await axios.put(url.value + '/finalizar', {id: idAtividade})
  .then(()=>
  {alert("Atividade Finalizada");
  getAtividades();
}).catch(()=>{
  alert("atividade não finalizada, tente novamente")
})
  

}
async function  deleteAtividade(atividade){
  let idAtividade = atividade.id;
  await axios.delete(url.value + "/" + idAtividade)
  .then(()=>getAtividades())
  .catch(()=>alert('erro ao deletar'))
}

const url = ref('http://localhost:3000/api/atividade')
const count = ref(0)
const rota = ref("home")
const nome = ref("")
const editar = ref({})
const items = ref([
  
  
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
              <v-form >
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
                    <v-btn type="button" @click="putAtividade(editar)" block class="mt-2">Salvar edição</v-btn>
                  </v-container>
                </v-form>
              </div>
        <div v-if="rota === 'novo'">

          <v-form >
              <v-container>
                <v-row>
                  <v-col
                    cols="12"
                    md="4"
                  >
                    <v-text-field
                      v-model="nome"
                      
                      :counter="10"
                      label="Nome da atividade"
                      required
                    ></v-text-field>
                  </v-col>   
                </v-row>
                <v-btn type="button" @click="createAtividade" block class="mt-2">Enviar atividade</v-btn>
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
                 <v-btn  @click="rota = 'editar';editar = item" type="button" icon="mdi-pencil"  width="40" ></v-btn>
                 <v-btn  @click="deleteAtividade(item)" type="button" icon="mdi-trash-can-outline"  width="40" ></v-btn></v-row>
                
              </template>
              <template v-slot:subtitle>
                Data de cadastro: {{ item.dataCadastro }}
              </template>
              <template v-slot:text>
                <p v-if="item.atvStatus === 'Finalizada'">Finalizada dia: {{ item.dataFinalizacao }} <v-icon color="blue">mdi-check-all</v-icon></p>
                <p v-else> Não finalizada <v-icon >mdi-check</v-icon></p>
              </template>
              <v-card-actions  v-if="item.status != 'Finalizada'">
                
                <v-btn 
                v-if="item.atvStatus !== 'Finalizada'" 
                @click="finalizar(item)" 
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
