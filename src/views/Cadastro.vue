<template>
  <div class="page">
    <div class="box">
      <h1>Cadastrar Disco</h1>

      <input v-model="nome" placeholder="Nome do Álbum" />
      <input v-model="artista" placeholder="Artista" />
      <input v-model="ano" placeholder="Ano" />
      <input v-model="imagem" placeholder="URL da Imagem" />

      <textarea v-model="descricaoArtista" placeholder="Descrição do Artista"></textarea>
      <textarea v-model="descricaoAlbum" placeholder="Descrição do Álbum"></textarea>

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
const imagem = ref('')
const descricaoArtista = ref('')
const descricaoAlbum = ref('')

const router = useRouter()

async function salvar() {
  if (!nome.value || !artista.value) {
    alert('Por favor, preencha o nome e o artista!')
    return
  }

  await api.post('/discos', {
    nome: nome.value,
    artista: artista.value,
    ano: Number(ano.value),
    imagem: imagem.value,
    descricaoArtista: descricaoArtista.value,
    descricaoAlbum: descricaoAlbum.value,
  })

  alert('Disco cadastrado com sucesso!')
  router.push('/')
}
</script>

<style>
.page {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background: #242424;
}

.box {
  background: #16427a;
  padding: 30px;
  border-radius: 10px;
  width: 750px;
}

input,
textarea {
  width: 98%;
  margin-bottom: 10px;
  padding: 8px;
}

textarea {
  min-height: 80px;
}

button {
  background: #3886ed;
  color: white;
  padding: 10px;
  border: none;
  border-radius: 6px;
}
</style>
