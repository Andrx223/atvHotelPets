<script setup>

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
  
  </div>
</template>
