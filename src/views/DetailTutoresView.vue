<script setup>
import { ref, onMounted } from 'vue';
import { useRoute } from 'vue-router';
const route = useRoute();

const tutor = ref(null);
const API_URL = 'http://localhost:3000';

async function carregarTutor(){
    //serve para pegar os números do final do navegador e pegar o tutor certo
    const idTutor = route.params.id;

    //chamando a API para busvar o pet pelo id
    const respostaTutor = await fetch(`${API_URL}/tutores/${idTutor}`)
    console.log('ResposTutor', respostaTutor);

    //convertendo a resposta da API para JSON
    tutor.value = await respostaTutor.json();
    console.log('RespostaTutor', tutor.value);



}
onMounted(carregarTutor)
</script>
<template>

    <section v-if="tutor === null">
        <p>Carregando.....</p>
    </section>


    <section v-else>

    <h1>Nome tutor: {{tutor?.nome}}   </h1>
    <p>ID: {{tutor?.id}}</p>
    <h5>Telefone: {{tutor.telefone}} </h5>
    </section>

</template>

<style scoped>

</style>