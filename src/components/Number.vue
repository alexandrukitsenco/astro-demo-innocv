<template>
  <div class="number-page">
    {{ paginacion + 1 }}
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted } from "vue";
import { navigate } from "astro:transitions/client";

const paginas = ref<string[]>(["/", "/portada", "/indice"]);
const paginaActual = ref(0);
const paginacion = ref(0);

const navegar = (event: KeyboardEvent) => {
  paginacion.value++;
  if (event.key === "ArrowRight") {
    paginaActual.value = (paginaActual.value + 1) % paginas.value.length;
    navigate(paginas.value[paginaActual.value]);
  } else if (event.key === "ArrowLeft") {
    paginaActual.value = (paginaActual.value - 1 + paginas.value.length) % paginas.value.length;
    navigate(paginas.value[paginaActual.value]);
  }
};

const manejarKeyDown = (event: KeyboardEvent) => {
  navegar(event);
};

onMounted(() => {
  document.addEventListener("keydown", manejarKeyDown);
});

onUnmounted(() => {
  document.removeEventListener("keydown", manejarKeyDown);
});
</script>

<style>
.number-page {
  font-size: 20px;
}
</style>
