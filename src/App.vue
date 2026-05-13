<script setup>
import { ref, computed } from 'vue';

const nombre = ref('');
const edad = ref(null);
const biografia = ref('');
const nivel = ref('');
const intereses = ref([]);
const pais = ref(null);
const tecnologias = ref([]);
const niveles = ['Junior', 'Semi Senior', 'Senior'];
const opciones = ['Vue', 'React', 'Angular', 'Svelte', 'Astro'];
const paises = [
  {
    code: 'CL',
    nombre: 'Chile',
  },
  {
    code: 'AR',
    nombre: 'Argentina',
  },
  {
    code: 'VE',
    nombre: 'Venezuela',
  },
];

const resumen = computed(() => ({
  nombre: nombre.value,
  edad: edad.value,
  biografia: biografia.value,
  nivel: nivel.value,
  intereses: intereses.value,
  pais: pais.value,
  tecnologias: tecnologias.value,
}));

// funcion para ver si alguno de los datos de registro deja de ser falsy (o si los arrays tienen 0 elementos)
const tieneDatos = computed(() => {
  return Object.values(resumen.value).some((valor) => {
    if (Array.isArray(valor)) return valor.length > 0;
    return Boolean(valor);
  });
});

const imprimirResumenConsola = (data) => {
  console.log('===================================');
  console.log('Resumen del registro:');
  console.group(JSON.stringify(data));
};
</script>

<template>
  <div class="container">
    <div class="row pt-5">
      <div class="col-12 col-lg-6">
        <h1 class="mb-4">Formulario de registro</h1>

        <form @submit.prevent="imprimirResumenConsola(resumen)">
          <!-- Nombre -->
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

          <!-- Edad -->
          <div class="mb-4">
            <label class="form-label">Edad</label>
            <input
              type="number"
              v-model.number="edad"
              class="form-control"
              :class="{
                'is-invalid': edad !== null && (edad < 0 || edad > 120),
              }"
              placeholder="Ingrese un número entre 0 y 120"
            />
            <!-- mensaje de error -->
            <small class="invalid-feedback">Debe ser un número entre 0 y 120</small>
          </div>

          <!-- Biografía -->
          <div class="mb-4">
            <label class="form-label">Biografía</label>
            <textarea v-model.lazy="biografia" class="form-control" rows="4"></textarea>
            <small class="text-muted">Caracteres: {{ biografia.length }}</small>
          </div>

          <!-- Nivel -->
          <div class="mb-4">
            <label class="form-label d-block">Nivel</label>
            <div v-for="n in niveles" :key="n" class="form-check form-check-inline">
              <input
                type="radio"
                :value="n"
                class="form-check-input"
                v-model="nivel"
                :id="`r-${n}`"
              />
              <label :for="`r-${n}`" class="form-check-label"> {{ n }}</label>
            </div>
          </div>

          <!-- Intereses -->
          <div class="mb-4">
            <label class="form-label d-block">Intereses</label>
            <div v-for="o in opciones" class="form-check form-check-inline" :key="o">
              <input
                type="checkbox"
                :value="o"
                :id="`c-${o}`"
                class="form-check-input"
                v-model="intereses"
              />
              <label :for="`c-${o}`" class="form-check-label">{{ o }}</label>
            </div>
          </div>

          <!-- País -->
          <div class="mb-4">
            <label class="form-label">País</label>
            <select class="form-select" v-model="pais">
              <option disabled :value="null">Selecciona un país</option>
              <option v-for="p in paises" :value="p" :key="p.code">{{ p.nombre }}</option>
            </select>
          </div>

          <!-- Tecnologías -->
          <div class="mb-4">
            <label class="form-label">Tecnolgías</label>
            <select class="form-select" v-model="tecnologias" multiple>
              <option v-for="o in opciones" :value="o" :key="o">{{ o }}</option>
            </select>
            <small>Puede seleccionar más de una opción</small>
          </div>

          <button type="submit" class="btn btn-primary mb-4">Enviar</button>
        </form>
      </div>
      <div class="col-12 col-lg-6">
        <h2 class="h1 mb-4">Vista previa</h2>
        <article class="card p-3 mb-5 shadow">
          <p v-if="!tieneDatos" class="mb-0 text-center">(Sin datos)</p>
          <ul v-else class="list-group list-group-flush">
            <li class="list-group-item list-group-item-warning">
              <strong>Nombre:</strong> {{ nombre }}
            </li>
            <li class="list-group-item list-group-item-warning">
              <strong>Edad:</strong> {{ edad }}
            </li>
            <li class="list-group-item list-group-item-warning">
              <strong>Biografía:</strong> {{ biografia }}
            </li>
            <li class="list-group-item list-group-item-warning">
              <strong>Nivel:</strong> {{ nivel }}
            </li>
            <li class="list-group-item list-group-item-warning">
              <strong>Intereses: </strong> {{ intereses.length < 1 ? '' : intereses.join(', ') }}
            </li>
            <li class="list-group-item list-group-item-warning">
              <strong>País:</strong> {{ pais !== null ? pais.nombre : '' }}
              {{ pais !== null ? `(${pais.code})` : '' }}
            </li>
            <li class="list-group-item list-group-item-warning">
              <strong>Tecnologías:</strong>
              {{ tecnologias.length < 1 ? '' : tecnologias.join(', ') }}
            </li>
          </ul>
        </article>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
