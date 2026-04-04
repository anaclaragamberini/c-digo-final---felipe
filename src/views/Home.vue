<template>
  <div class="page">
    <div class="box">
      <h1>Lista de Discos</h1>

      <ul>
        <li v-for="disco in discos" :key="disco.id">
          {{ disco.nome }} - {{ disco.artista }} ({{ disco.ano }})
          <div>
            <button @click="editar(disco.id)">Editar</button>
            <button @click="excluir(disco.id)">Excluir</button>
            <button @click="verDetalhes(disco)">Ver detalhes</button>
          </div>
        </li>
      </ul>

      <div v-if="mostrarModal" class="modal-overlay">
        <div class="modal">
          <h2>{{ discoSelecionado.nome }}</h2>
          <hr />

          <h3>Sobre o artista</h3>
          <p>{{ discoSelecionado.descricaoArtista || 'Nenhuma descrição do artista informada.' }}</p>

          <h3>Sobre o álbum</h3>
          <p>{{ discoSelecionado.descricaoAlbum || 'Nenhuma descrição do álbum informada.' }}</p>

          <button class="btn-fechar" @click="mostrarModal = false">Fechar</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { useRouter } from 'vue-router'
import api from '@/services/api'

const discos = ref([])
const router = useRouter()

const mostrarModal = ref(false)
const discoSelecionado = ref(null)

function verDetalhes(disco) {
  discoSelecionado.value = disco
  mostrarModal.value = true
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
  width: 450px;
}

li {
  margin-bottom: 10px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-bottom: 1px solid #eee;
  padding-bottom: 5px;
}

button {
  margin-left: 5px;
  cursor: pointer;
}

/* Estilos do Modal Estilo Sakai */
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.6);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 9999;
}

.modal {
  background: white;
  padding: 25px;
  border-radius: 12px;
  width: 400px;
  text-align: center;
  box-shadow: 0 4px 20px rgba(0,0,0,0.3);
}

.modal h2 {
  margin-top: 0;
  color: #333;
}

.modal h3 {
  font-size: 1.1rem;
  color: #6366f1; /* Cor primária do Sakai */
  margin-top: 15px;
  margin-bottom: 5px;
}

.modal p {
  font-size: 0.95rem;
  line-height: 1.4;
  color: #555;
  white-space: pre-wrap; /* Mantém as quebras de linha que você digitar */
}

.btn-fechar {
  margin-top: 20px;
  background: #6366f1; /* Cor do Sakai */
  color: white;
  border: none;
  padding: 10px 20px;
  border-radius: 5px;
  width: 100%;
}

.btn-fechar:hover {
  background: #4f46e5;
}
</style>