<template>
  <carousel :items-to-show="3">
    <slide v-for="character in characters" :key="character.id">
      <img :src="character.image" :alt="character.name" />
    </slide>

    <template #addons>
      <navigation />
      <pagination />
    </template>
  </carousel>
</template>

<script>
import "vue3-carousel/dist/carousel.css";
import { Carousel, Slide, Pagination, Navigation } from "vue3-carousel";
import { ref, onMounted } from "vue";

export default {
  name: "App",
  components: {
    Carousel,
    Slide,
    Pagination,
    Navigation,
  },
  setup() {
    const characters = ref([]);

    const fetchCharacters = async () => {
      try {
        const response = await fetch("https://rickandmortyapi.com/api/character");
        const data = await response.json();
        characters.value = data.results;
      } catch (error) {
        console.error("Error fetching characters:", error);
      }
    };

    onMounted(fetchCharacters);

    return { characters };
  },
};
</script>
