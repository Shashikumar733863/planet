<template>
  <div class="m-2 shadow-md p-3 bg-slate-50 rounded-md ">
    <h3 class="font-extrabold text-lg">Planet Name: {{ planet.name }}</h3>
    <p><strong>Climate:</strong> {{ planet.climate }}</p>
    <p><strong>Population:</strong> {{ planet.population }}</p>
    <p><strong>Terrain:</strong> {{ planet.terrain }}</p>
    <div v-if="planet.residents.length > 0">
      <h4>Residents:</h4>
      <ul>
        <li v-for="(resident, index) in residents" :key="resident.name">
          {{ resident.name }} (Height: {{ resident.height }}), Mass: {{ resident.mass }}, Gender: {{ resident.gender }})
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    planet: {
      type: Object,
      required: true,
    },
  },
  data() {
    return {
      residents: [],
    };
  },
  mounted() {
    this.fetchResidents(this.planet.residents);
  },
  methods: {
    async fetchResidents(residentUrls) {
      const residents = [];
      for (const url of residentUrls) {
        try {
          const response = await fetch(url);
          if (!response.ok) {
            console.error("Error fetching resident:", url, response.status);
            continue;
          }
          const data = await response.json();
          residents.push(data);
        } catch (error) {
          console.error("Error fetching resident:", url, error);
        }
      }
      this.residents = residents;
    },
  },
};
</script>

<style scoped>


.planet-card {
  background-color: #fff;
  border: 1px solid #ddd;
  border-radius: 5px;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
  padding: 2rem;
  margin: 1rem;
  display: flex;
  flex-direction: column;
  gap: 1rem;

}
.planet-card h3 {
  font-size: 1.5rem;
  margin: 0;
  font-weight: bold;
  position: relative;
  overflow: hidden;

  transition: transform 0.2s ease-in-out;

  &.animated {
    transform: translateY(-10px);
  }
}
 
</style>
