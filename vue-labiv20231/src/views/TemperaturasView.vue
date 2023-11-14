<template>
  <div class="about">
    <h1>Cadastro</h1>
    <label for="dataHora">Date and Time: </label>
    <input id="dataHora" type="datetime-local" v-model="dataHora">
    <p></p>
    
    <label for="medida">Medida: </label>
    <input id="medida" type="number" v-model="medida" /> 
    <p></p>

    <label for="umidade">Umidade: </label>
    <input id="umidade" type="number" v-model="umidade" />
    <p></p>

    <label for="vento">Vento: </label>
    <input id="vento" type="number" v-model="vento" />
    <p></p>

    <label for="particulas">Particulas: </label>
    <input id="particulas" type="number" v-model="particulas" />
    <p></p>

    <button @click="cadastrar">Cadastrar</button>

    <h1>Buscar por Data/Hora Maior e Medida Maior</h1>
    <button @click="buscarPorDataHoraMaiorMedidaMaior">Buscar</button>

    <p v-if="erro_busca">Nenhum registro foi encontrado para os critérios fornecidos.</p>

    <p v-if="erro">{{ erro }}</p>

    <table>
      <thead>
        <td>Id</td>
        <td>Data/Hora</td>
        <td>Medida</td>
        <td>Sensação</td>
      </thead>
      <tr v-for="temp in temperaturas" :key="temp.id">
        <td>{{ temp.id }}</td>
        <td>{{ temp.dataHora }}</td>
        <td>{{ temp.medida }}</td>
        <td v-if="temp.medida > 26">Quente</td>
        <td v-else>Ok</td>
      </tr>
    </table>
  </div>
</template>

<script setup lang="ts">
  import { onMounted, ref } from 'vue';
  import axios from 'axios';

  const dataHora =  ref();
  const medida = ref();
  const umidade =  ref();
  const vento = ref();
  const particulas = ref();
  const temperaturas = ref();
  const erro = ref();
  const erro_busca = ref();
  
  async function atualizar() {
    try { 
      temperaturas.value = (await axios.get("temperatura")).data;
    }
    catch(ex) {
      erro.value = (ex as Error).message;
    }
  }

  onMounted(() => {
    atualizar();
  });

  async function buscarPorDataHoraMaiorMedidaMaior() {
    try { 
      temperaturas.value = (await axios.get("temperatura/" + dataHora.value + '/' + medida.value)).data;
      console.log(temperaturas.value)
    }
    catch(ex) {
      erro_busca.value = 'Nenhum registro foi encontrado para os critérios fornecidos.';
    }
  }

  async function cadastrar() {
    try {
      await axios.post("temperatura",
      {
        "dataHora": dataHora.value,
        "medida": medida.value,
        "umidade": umidade.value,
        "vento": vento.value,
        "particulas": particulas.value
      });
      dataHora.value = ""
      medida.value = ""
      umidade.value = ""
      vento.value = ""
      particulas.value = ""
    }
    catch(ex) {
      erro.value =(ex as Error).message;
    }
    atualizar();
  }
</script>