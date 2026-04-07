<template>
  <div class="page">
    <div class="box">
      <h1>Lista de Discos</h1>

      <ul>
        <li v-for="disco in discos" :key="disco.id">
          <img v-if="disco.imagem" :src="disco.imagem" alt="Capa" />

          {{ disco.nome }} - {{ disco.artista }} ({{ disco.ano }})

          <div>
            <button @click="editar(disco.id)">Editar</button>
            <button @click="excluir(disco.id)">Excluir</button>
            <button @click="verDetalhes(disco)">Ver detalhes</button>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { useRouter } from 'vue-router'
import api from '@/services/api'

const discos = ref([])
const router = useRouter()

function verDetalhes(disco) {
  alert(
    `Nome: ${disco.nome}
Artista: ${disco.artista}
Ano: ${disco.ano}

Sobre o artista:
${disco.descricaoArtista || 'Nenhuma descrição do artista informada.'}

Sobre o álbum:
${disco.descricaoAlbum || 'Nenhuma descrição do álbum informada.'}`,
  )
}

async function carregar() {
  const res = await api.get('/discos')
  discos.value = res.data
}

function editar(id) {
  router.push(`/editar/${id}`)
}

async function excluir(id) {
  const confirmar = window.confirm('Tem certeza que deseja excluir este disco?')
  if (!confirmar) return
  await api.delete(`/discos/${id}`)
  carregar()
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
  padding: 10px;
}

.box {
  background: #16427a;
  padding: 30px;
  border-radius: 10px;
  box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.5);
  width: 750px;
}

h1 {
  margin-bottom: 40px;
  text-align: center;
  color: whitesmoke;
  font-family: Verdana, Geneva, Tahoma, sans-serif;
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

img {
  width: 90px;
  height: 90px;
  object-fit: cover;
  border-radius: 5px;
  box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.5);
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

button:last-child {
  background: #c93c4a;
  color: whitesmoke;
}
</style>
