<template>
  <div class="page">
    <div class="box">
      <h1>Editar Disco</h1>

      <input v-model="nome" placeholder="Nome" />
      <input v-model="artista" placeholder="Artista" />
      <input v-model="ano" placeholder="Ano" />
      <input v-model="imagem" placeholder="Imagem" />

      <textarea v-model="descricaoArtista" placeholder="Descrição do Artista"></textarea>
      <textarea v-model="descricaoAlbum" placeholder="Descrição do Álbum"></textarea>

      <button @click="atualizar">Atualizar</button>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { useRoute, useRouter } from 'vue-router'
import api from '@/services/api'

const route = useRoute()
const router = useRouter()
const id = route.params.id

const nome = ref('')
const artista = ref('')
const ano = ref('')
const imagem = ref('')
const descricaoArtista = ref('')
const descricaoAlbum = ref('')

async function carregar() {
  const res = await api.get(`/discos/${id}`)

  nome.value = res.data.nome
  artista.value = res.data.artista
  ano.value = res.data.ano
  imagem.value = res.data.imagem
  descricaoArtista.value = res.data.descricaoArtista
  descricaoAlbum.value = res.data.descricaoAlbum
}

async function atualizar() {
  await api.put(`/discos/${id}`, {
    nome: nome.value,
    artista: artista.value,
    ano: Number(ano.value),
    imagem: imagem.value,
    descricaoArtista: descricaoArtista.value,
    descricaoAlbum: descricaoAlbum.value,
  })

  alert('Atualizado!')
  router.push('/')
}

onMounted(carregar)
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
