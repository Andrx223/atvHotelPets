<script setup>
import { onMounted, ref } from 'vue';
import { RouterLink } from 'vue-router';


const pets = ref([]); // tabela pets vazia
const tutores = ref([]); // tabela tutores vazia


//chamando minha api geral
const API_URL = 'http://localhost:3000'

// fetch == chamar o servidor X
//chmar minha api para listar todos os petse tutores
async function carregarDados(){
    const respostaPets = await fetch(`${API_URL}/pets`)
    pets.value = await respostaPets.json();

    const respostaTutores = await fetch(`${API_URL}/tutores`)
    tutores.value = await respostaTutores.json();

    console.log('PETS -' , pets.value);
    console.log('TUTORES -' , tutores.value);
}
//exibe nome do tutor
function nomeDoTutor(tutorId){
for(const tutor of tutores.value){
    if(tutor.id === tutorId){
        return tutor.nome;
    }
}
return 'Ops, Tutor não encontrado'
}


async function excluirPet(id){
    const confirmarExclusao = window.confirm('Deseja excluir realmente excluir o pet?');

    if(confirmarExclusao){
        //logica para excluir o pet http://localhost:3000/pets/${id}
        await fetch(`${API_URL}/pets/${id}`,{
            method: 'DELETE',
        });
        await carregarDados()
    }
}
onMounted(carregarDados)
</script>
<template>
  <div>
    <header class="mb-4">
      <h1 class="text-2xl font-bold">Listagem de Pets</h1>
      <p class="text-body-secondary mb-0">
        Listagem dos Pets cadastrados no sistema.
      </p>
    </header>

    <table class="table table-striped table hover">
        <thead>
            <th>id</th>
            <th>Nome</th>
            <th>Espécie</th>
            <th>Tutor</th>
            <th>Ações</th>
        </thead>
        <tbody>
            <tr v-for="pet in pets" :key="pet.id">
                <td>{{ pet.id }}</td>
                <td>{{ pet.nome }}</td>
                <td>{{ pet.especie }}</td>
                <td>{{ nomeDoTutor(pet.tutorId) }}</td>
                <td>
                    <RouterLink :to="`/pets/${pet.id}`"> Visualizar</RouterLink>
                    <button class="btn btn-danger" @click="excluirPet(pet.id)">Excluir</button>
                </td>
            </tr>
        </tbody>
    </table>
    </div>
    </template>