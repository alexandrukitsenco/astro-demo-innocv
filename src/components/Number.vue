<template>
  <div v-show="paginacion >= 0" class="number-page">
    {{ paginacion + 1 }}
  </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted } from "vue";
import { navigate } from "astro:transitions/client";

const paginas = ref<string[]>([
  "/",
  "/indice",
  "/intro",
  "/arquitectura",
  "/desarrollo",
  "/case-use",
  "/community",
  "/preguntas",
]);
const paginaActual = ref(0);
const paginacion = ref(-1);

const manejarNavegacion = (event: string) => {
  if (event === "ArrowRight" && paginaActual.value < paginas.value.length - 1) {
    navegar(true);
  } else if (event === "ArrowLeft" && paginaActual.value > 0) {
    navegar(false);
  }
};

const navegar = (direction: boolean) => {
  // true is right and false is left
  if (direction) {
    paginaActual.value++;
  } else {
    paginaActual.value--;
  }
  paginacion.value = paginaActual.value - 2;
  navigate(paginas.value[paginaActual.value]);
};

const manejarKeyDown = (event: KeyboardEvent) => {
  if (event.key === "ArrowRight" || event.key === "ArrowLeft") {
    manejarNavegacion(event.key);
  }
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
