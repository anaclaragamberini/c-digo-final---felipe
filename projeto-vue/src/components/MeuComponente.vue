<template>
    <div >
        <h1> {{ titulo }} </h1>

        <p> {{ contador }} </p>

        <p v-if="contador < 10"> O contador é menor que 10 </p><!-- condição if que verifica se contador é menor que 10 -->
        <p v-else> O contador é igual ou maior que 10 </p>

        <a :href="link" target="_blank"> Clique aqui para ir para o Google </a> <!-- v-bind para passar link dinâmico para atributo href -->

        <button @click="onButtonClick">Aumentar contador</button> <!-- Escuta evento de clique do botão -->

        <div :class="estiloContador" @mouseenter="onMouseEnter" @mouseleave="onMouseOut"><!-- estilo diâmico @mouseenter escuta evento de mouse em cima da div @mouseleave quando mouse sai-->
            <p v-for="i in contador"> {{ i }} </p> <!-- laço for para parágrafo -->
        </div>
    </div>
</template>

<script setup>
import { ref } from 'vue'

const titulo = ref('Meu componente Vue.')
const contador = ref(0)
const diasDaSemana = ref(["Dom", "Seg", "Ter", "Qua", "Qui", "Sex", "Sab"])
const link = ref('https://google.com')
const estiloContador = ref('contador')

//Define uma propriedade que pode ser recebida por um componente externo.
const props = defineProps({
    maximo: Number
})

//Declara um evento que poderá ser emitido por este componente e escutado por um componente externo.
const emit = defineEmits(['atingiuValorMaximo'])

function onButtonClick() {

    if(contador.value < props.maximo) {
        contador.value++
    }

    if (contador.value == props.maximo) {
        emit('atingiuValorMaximo', true)//Momento em que o evento é emitido
    }

    if (contador.value > 10) {
        estiloContador.value = 'contador-vermelho'
    }
}

function onMouseEnter() {
    estiloContador.value = 'contador-verde'
}


function onMouseOut() {
    estiloContador.value = 'contador'
}

</script>

<style>
.contador {
    background-color: aqua;
}

.contador-vermelho {
    background-color: red;
}

.contador-verde {
    background-color: green;
}

</style>