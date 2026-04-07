<template>
  <div class="page">
    <div class="box">
      <h1>Cadastrar Disco</h1>

      <input v-model="nome" placeholder="Nome" />
      <input v-model="artista" placeholder="Artista" />
      <input v-model="ano" placeholder="Ano" />
      <input v-model="imagem" placeholder="URL da Imagem" />

      <button @click="salvar">Salvar</button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import api from '@/services/api'

const nome = ref('')
const artista = ref('')
const ano = ref('')
const imagem = ref('')
const router = useRouter()

async function salvar() {
  await api.post('/discos', {
    nome: nome.value,
    artista: artista.value,
    ano: Number(ano.value),
    imagem: imagem.value,
  })

  alert('✅ Disco cadastrado com sucesso!')
  router.push('/')
}
</script>

<style>

.container {
  padding: 0px;
}

.page {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background: #242424;
  padding: 10px;
}

.box {
  background: #16427a;
  padding: 30px;
  border-radius: 10px;
  box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.5);
  width: 750px;
  align-content: center;
}

nav {
  background: #16427a;
  color: whitesmoke;
  font-family: Verdana, Geneva, Tahoma, sans-serif;
}

h1 {
  margin-bottom: 40px;
  text-align: center;
  color: whitesmoke;
  font-family: Verdana, Geneva, Tahoma, sans-serif;
}

li {
  font-family: Verdana, Geneva, Tahoma, sans-serif;
}

input {
  width: 98%;
}

img {
  width: 90px; 
  height: 90px; 
  object-fit: cover; 
  border-radius: 5px;
  box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.5);
}

ul {
  list-style: none;
  padding: 0;
  margin: 0;
  color: whitesmoke;
}

li {
  margin-bottom: 12px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 10px;
  padding: 10px;
  border-bottom: 1px solid #eee;
}

button {
  padding: 10px 15px;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-family: Verdana, Geneva, Tahoma, sans-serif;
}

button:first-child {
  background: #3886ed;
  color: whitesmoke;
}

button:first-child:hover {
  background: rgb(32, 92, 170);
}

button:last-child {
  background: #c93c4a;
  color: whitesmoke;
}

button:last-child:hover {
  background: #a8333f
}

.sem-resultados {
  text-align: center;
  color: #777;
  margin-top: 15px;
}
</style>
