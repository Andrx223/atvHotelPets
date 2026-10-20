<script setup>
import { onMounted, ref } from 'vue';
import { RouterLink, useRouter } from 'vue-router';

const router = useRouter(); //chamando meu router
const API_URL = 'http://localhost:3000';

const tutores = ref([]); //lista vazia dos meus tutores

// cria objeto para salvar outo pet
// propiedades: nome, especie e tutorIdd
const novoTutor = ref({
  nome: '',
  telefone: '',
  id: ''
});

//buscar todos os tutores da minha aplicação salvos

async function carregarTutores() {
  const resposta = await fetch(`${API_URL}/tutores`);
  //converter para .json
  tutores.value = await resposta.json();
  console.table(tutores);
}

// criar function salvar pet
async function salvarTutor() {
  await fetch(`${API_URL}/tutores`, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify(novoTutor.value),
  });

  //redicirecionar para a tela de listagem
  router.push('/tutores');
}

onMounted(carregarTutores);
</script>
<template>
  <div>
    <header class="mb-4">
      <h1 class="text-2xl font-bold">Listagem de Tutores</h1>
      <p class="text-body-secondary mb-0">
        Listagem dos tutores cadastrados no sistema
      </p>
    </header>

    <Routerlink
      class="btn btn-primary"
      :to="{ name: 'novo-tutor' }"
    >
      Adcionar Tutor
    </Routerlink>
    <form
      class="row"
      @submit.prevent="salvarTutor"
    >
      <!-- nome do pet -->
      <div class="col-md-">
        <label
          for="nome"
          class="form-label"
        >
          Nome do Tutor:
        </label>
        <input
          type="text"
          class="form-control"
          id="nome"
          v-model="novoTutor.nome"
          required
        />
      </div>
      <!-- telefone do tutor -->
     <div class="col-md-">
        <label
          for="nome"
          class="form-label"
        >
          Telefone do Tutor:
        </label>
        <input
          type="text"
          class="form-control"
          id="nome"
          v-model="novoTutor.telefone"
          required
        />
      </div>
     

      <div class="col-12 d-flex gap-2">
        <button class="btn btn-success">Salvar Tutor</button>
      </div>
    </form>
    <!-- fazer post dos tutores nome email e telefone -->
  </div>
</template>
