<template>
  <div class="about">
    <h1>Bem vindo {{ namezin }}</h1>
    <label for="nome">Nome: </label>
    <input id="nome" type="text" v-model="nome" /> 
    <label for="email">Email: </label>
    <input id="email" type="text" v-model="email" /> 
    <label for="ctps">ctps: </label>
    <input id="ctps" type="number" v-model="ctps" />
    <label for="dataCadastro">Data Cadastro: </label>
    <input id="dataCadastro" type="text" v-model="dataCadastro" />
    <label for="cargaHoraria">Carga Horaria: </label>
    <input id="cargaHoraria" type="text" v-model="cargaHoraria" />
    <button @click="cadastrar">Cadastrar</button>

    <p v-if="erro">{{ erro }}</p>

    <table>
      <thead>
        <td>Id</td>
        <td>Nome</td>
        <td>Email</td>
      </thead>
      <tr v-for="usuario in usuarios" :key="usuario.id">
        <td>{{ usuario.id }}</td>
        <td>{{ usuario.nome }}</td>
        <td>{{ usuario.email }}</td>
      </tr>
    </table>
  </div>
</template>

<script setup lang="ts">
  import { onMounted, ref } from 'vue';
  import axios from 'axios';

  const nome =  ref("Nomezin");
  const email = ref("teste1@gmail.com");
  const ctps =  ref(1235456);
  const dataCadastro = ref("2023-11-13T13:04:00");
  const cargaHoraria = ref(null);
  const usuarios = ref();
  const erro = ref();
  
  async function atualizar() {
    try { 
      usuarios.value = (await axios.get("empregado")).data;
    }
    catch(ex) {
      erro.value = (ex as Error).message;
    }
  }

  onMounted(() => {
    atualizar();
  });


  async function cadastrar() {
    try {
      await axios.post("empregado",
      {
        "nome": nome.value,
        "email": email.value,
        "ctps": ctps.value,
        "dataCadastro": dataCadastro.value,
        "cargaHoraria": cargaHoraria.value
      });
    }
    catch(ex) {
      erro.value =(ex as Error).message;
    }
    atualizar();
  }
</script>