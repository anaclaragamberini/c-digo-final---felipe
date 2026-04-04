<template>
  <div class="page">
    <div class="box">
      <h1>Cadastrar Disco</h1>

      <input v-model="nome" placeholder="Nome do Álbum" />
      <input v-model="artista" placeholder="Artista" />
      <input v-model="ano" placeholder="Ano de Lançamento" />
      
      <textarea v-model="descricaoArtista" placeholder="Sobre o Artista..."></textarea>
      <textarea v-model="descricaoAlbum" placeholder="Sobre o Álbum..."></textarea>

      <button @click="salvar">Salvar Disco</button>
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
const descricaoArtista = ref('') 
const descricaoAlbum = ref('')   
const router = useRouter()

async function salvar() {
  if (!nome.value || !artista.value) {
    alert('Por favor, preencha o nome e o artista!');
    return;
  }

  // O erro deve estar aqui embaixo:
  await api.post('/discos', {
    nome: nome.value,              
    artista: artista.value,        
    ano: Number(ano.value),   
    descricaoArtista: descricaoArtista.value, 
    descricaoAlbum: descricaoAlbum.value       
  })

  alert('Disco cadastrado com sucesso!')
  router.push('/')
}
</script>

<style>
.page {
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background: #f5f5f5;
}

.box {
  background: white;
  padding: 30px;
  border-radius: 10px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  display: flex;
  flex-direction: column;
  gap: 12px;
  width: 350px; /* Aumentei um pouco para os textareas */
}

input, textarea {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
  font-family: sans-serif;
}

textarea {
  height: 80px;
  resize: none; /* Impede o usuário de bagunçar o layout */
}

button {
  background-color: #6366f1;
  color: white;
  border: none;
  padding: 12px;
  border-radius: 5px;
  cursor: pointer;
  font-weight: bold;
}

button:hover {
  background-color: #4f46e5;
}
</style>