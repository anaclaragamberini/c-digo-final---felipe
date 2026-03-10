<script setup>
import MeuComponente from './components/MeuComponente.vue';
import { ref } from 'vue';

const valorMaximo = ref(12)
const valorMaximoAtingido = ref(false)

function onValorMaximo(atingiuValorMaximo) {
  if(atingiuValorMaximo) {
    valorMaximoAtingido.value = true
  }
}

</script>
<template>
  <h3 v-show="valorMaximoAtingido"> Valor máximo atingido </h3>
  <MeuComponente :maximo="valorMaximo" @atingiu-valor-maximo="onValorMaximo"><!-- Parâmetros passados para as propriedades(props) do Componente customizado -->
  </MeuComponente>
</template>

<style scoped>
</style>