<script>
import PlanetCard from "./components/PlanetCard.vue";
import Pagination from "./components/Pagination.vue";

export default {
  components: {
    PlanetCard,
    Pagination,
  },
  data() {
    return {
      planets: [],
      currentPage: 1,
      totalPages: 1,
      itemsPerPage: 10, // Number of planets to show per page
    };
  },
  computed: {
    // Calculate the paginated planets based on the currentPage
    paginatedPlanets() {
      //  const startIndex = (this.currentPage - 1) * this.itemsPerPage;
      // const endIndex = startIndex + this.itemsPerPage;
      return this.planets.slice(0, 10);
    },
  },
  mounted() {
    this.fetchPlanets();
  },
  methods: {
    async fetchPlanets() {
      try {
        const response = await fetch(
          `https://swapi.dev/api/planets/?pformat=json&page=${this.currentPage}`
        );
        const data = await response.json();
        this.planets = data.results;
        this.totalPages = Math.ceil(data.count / this.itemsPerPage);
      } catch (error) {
        console.error("Error fetching planets:", error);
      }
    },
    async changePage(newpage) {
      this.currentPage = newpage;
      await this.fetchPlanets();
    },
  },
};
</script>
<template>
  <div>
    <div class="grid grid-cols-12 gap-2 m-2">
      <span class="md:col-span-3
        col-span-12" v-for="planet in paginatedPlanets" :key="planet.name">
        <planet-card :planet="planet" />
      </span>
    </div>

    <pagination
      :current-page="currentPage"
      :total-pages="totalPages"
      @page-change="changePage"
    />
  </div>
</template>

<style scoped>

</style>
