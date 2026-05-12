<script setup>
import { ref, computed } from 'vue';

const nombre = ref('');

const resumen = computed(() => ({
  nombre: nombre.value,
}));
</script>

<template>
  <div class="container">
    <div class="row">
      <div class="col-12 col-lg-6">
        <h1 class="mb-4">Formulario de registro</h1>

        <form>
          <div class="mb-4">
            <label for="registroNombre" class="form-label">Nombre</label>
            <input
              type="text"
              class="form-control"
              id="registroNombre"
              placeholder="Ingrese un nombre"
              v-model.trim="nombre"
            />
          </div>
        </form>
      </div>
      <div class="col-12 col-lg-6">
        <h2 class="h1">Vista previa</h2>

        <p>{{ nombre }}</p>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
