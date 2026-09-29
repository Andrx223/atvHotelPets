<script setup>  
  import { onMounted, ref } from 'vue';
  import { RouterLink, useRouter } from 'vue-router';

const router = useRouter(); //chamando meu router
const API_URL = 'http://localhost:3000';

const tutores = ref([]); //lista vazia dos meus tutores

// cria objeto para salvar outo pet 
// propiedades: nome, especie e tutorIdd
const novoPet = ref({
  nome:'',
  especie: '',
  tutorId: ''
});

//buscar todos os tutores da minha aplicação salvos

async function carregarTutores(){
  const resposta = await fetch(`${API_URL}/tutores`);
  //converter para .json
  tutores.value = await resposta.json();
  console.table(tutores)
}

// criar function salvar pet
async function salvarPet(){
await fetch(`${API_URL}/pets`, {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify(novoPet.value)
})

//redicirecionar para a tela de listagem
router.push('/pets')
}



  onMounted(carregarTutores)
</script>
<template>
    <div>

    
  <header class="mb-4">
    <h1 class="text-2xl font-bold">Listagem de Pets</h1>
    <p class="text-body-secondary mb-0">
      Listagem dos pets cadastrados no sistema
    </p>
  </header>

  <Routerlink
    class="btn btn-primary"
    :to="{ name: 'novo-pet' }"
  >
    Adcionar Pet
  </Routerlink>
  <form class="row" @submit.prevent="salvarPet">
  
<!-- nome do pet -->
    <div class="col-md-">
      <label for="nome" class="form-label">
        Nome do pet:
      </label>
      <input type="text" class="form-control" id="nome" v-model="novoPet.nome" required>
    </div>
<!-- especie do pet -->
    <div class="col-md-6">
      <label for="especie">Espécie</label>
      <select name="especie" id="especie" class="form-select" required v-model="novoPet.especie">
        <option value="" disabled>Selecone uma espécie</option>
        <option value="Cachorro">Cachorro</option>
        <option value="Gato">Gato</option>
        <option value="Coelho">Coelho</option>
        <option value="Tartaruga">Tartaruga</option>
      </select>
    </div>
<!-- tutor do pet -->
<div class="col-md-6">
  <label for="tutor" class="form-label">Tutor</label>
  <select name="tutor" id="tutor" class="form-select" v-model="novoPet.tutorId">

    <option value="" disabled>Selecione o tutor</option>
    <option v-for="tutor in tutores" :key="tutor.id">{{ tutor.nome }}</option>
  </select>
</div>

<div class="col-12 d-flex gap-2">
  <button class="btn btn-success">Salvar Pet</button>
</div>
  </form>
  <!-- fazer post dos tutores nome email e telefone -->
  </div>
</template>
