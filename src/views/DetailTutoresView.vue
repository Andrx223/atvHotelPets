<script setup>
import { ref, onMounted } from 'vue';
import { useRoute } from 'vue-router';
import router from '@/router';
const route = useRoute();
const petsDoTutor = ref(null);
const idTutor =  route.params.id;
const novoPet = {
    nome: '',
    especie: '',
    tutorId: idTutor
}
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

async function salvarPet() {
  await fetch(`${API_URL}/pets`, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify(novoPet),
  });

  //redicirecionar para a tela de listagem
  router.push('/pets');
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

    <h1>
        Adcionar um pet para este tutor
    </h1>
 <form
      class="row"
      @submit.prevent="salvarPet"
    >
      <!-- nome do pet -->
      <div class="col-md-">
        <label
          for="nome"
          class="form-label"
        >
          Nome do pet:
        </label>
        <input
          type="text"
          class="form-control"
          id="nome"
          v-model="novoPet.nome"
          required
        />
      </div>
      <!-- especie do pet -->
      <div class="col-md-6">
        <label for="especie">Espécie</label>
        <select
          name="especie"
          id="especie"
          class="form-select"
          required
          v-model="novoPet.especie"
        >
          <option
            value=""
            disabled
          >
            Selecone uma espécie
          </option>
          <option value="Cachorro">Cachorro</option>
          <option value="Gato">Gato</option>
          <option value="Coelho">Coelho</option>
          <option value="Tartaruga">Tartaruga</option>
        </select>
      </div>
    
      <div class="col-12 d-flex gap-2">
        <button class="btn btn-success">Salvar Pet</button>
      </div>
    </form>
    <!-- fazer post dos tutores nome email e telefone -->
  


</template>

<style scoped>

</style>