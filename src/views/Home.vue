<template>
  <div class="page">
    <div class="box">
      <h1>Lista de Discos</h1>

      <ul>
        <li v-for="disco in discos" :key="disco.id">

          <img 
            v-if="disco.imagem" 
            :src="disco.imagem" 
            alt="Capa"
          />

          {{ disco.nome }} - {{ disco.artista }} ({{ disco.ano }})

          <div>
            <button @click="editar(disco.id)">Editar</button>
            <button @click="excluir(disco.id)">Excluir</button>
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