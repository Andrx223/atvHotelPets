<script setup>
import { onMounted, ref } from 'vue';
import { RouterLink } from 'vue-router';


const tutores = ref(null);
// minha urllllllll
const API_URL = 'http://localhost:3000';


//function para acrregar os dados podres pelo fetch e por uma async
// async -> serve pra poder o await
// await -> serve pra fazer o código esperar e n ir pr apróxima linha no code
// fetch -> ele que vai la no link que eu passo e pega os dados brutos
// .json pq o pc n sabe ler nada alem de json
async function carregarDados(){
    const respostaTutores = await fetch(`${API_URL}/tutores`)
    tutores.value = await respostaTutores.json()
}

async function excluirTutor(id){
    const confirmarExclusao = window.confirm('Deseja excluir realmente excluir o tutor?');

    if(confirmarExclusao){
        //logica para excluir o pet http://localhost:3000/pets/${id}
        await fetch(`${API_URL}/tutores/${id}`,{
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
      <h1 class="text-2xl font-bold">Listagem de Tutores</h1>
      <p class="text-body-secondary mb-0">
        Listagem dos tutores cadastrados no sistema.
      </p>
    </header>

    <table class="table table-striped table hover">
        <thead>
            <th>id</th>
            <th>Nome</th>
            <th>Telefone</th>
        </thead>
        <tbody>
            <tr v-for="tutor in tutores" :key="tutor.id">
                <td>{{ tutor?.id }}</td>
                <td>{{ tutor.nome }}</td>
                <td>{{ tutor.telefone }}</td>
                <td>{{ tutor.email }}</td>
                <td>{{ tutor.cpf }}</td>
                <td>
                    <RouterLink :to="`/tutores/${tutor.id}`"> Visualizar</RouterLink>
                    <button class="btn btn-danger" @click="excluirTutor(tutor.id)">Excluir</button>
                </td>
            </tr>
        </tbody>
    </table>
    </div>
  
</template>



<style scoped>

</style>