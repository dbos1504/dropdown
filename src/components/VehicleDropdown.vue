<template>
  <div class="vehicle-dropdown">
    <div>
      <label for="year">Year</label>
      <select id="year" v-model="selectedYear" @change="fetchMakes">
        <option value="" disabled>Select Year</option>
        <option v-for="year in years" :key="year" :value="year">{{ year }}</option>
      </select>
    </div>

    <div v-if="makes.length > 0">
      <label for="make">Make</label>
      <select id="make" v-model="selectedMake" @change="fetchModels">
        <option value="" disabled>Select Make</option>
        <option v-for="make in makes" :key="make" :value="make">{{ make }}</option>
      </select>
    </div>

    <div v-if="models.length > 0">
      <label for="model">Model</label>
      <select id="model" v-model="selectedModel">
        <option value="" disabled>Select Model</option>
        <option v-for="model in models" :key="model" :value="model">{{ model }}</option>
      </select>
    </div>

    <div v-if="selectedModel">
      <p>You selected: {{ selectedYear }} - {{ selectedMake }} - {{ selectedModel }}</p>
    </div>

    <div v-if="error" class="error">
      <p>{{ error }}</p>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      years: [],
      makes: [],
      models: [],
      selectedYear: "",
      selectedMake: "",
      selectedModel: "",
      error: null,
    };
  },
  methods: {
    async fetchYears() {
      try {
        await axios.get("https://rateengine.ship.cars/v2/vehicles/years/?token=5cbe12fb62f4941267d623499a2a4fd5948fd3ef",
            { headers: { Accept: "application/json", "Content-Type": "application/json" } }
        ).then(({data}) => {
          this.years = data || [];
        });
      } catch (err) {
        this.error = "Failed to load years. Please try again later.";
        console.error(err);
      }
    },
    async fetchMakes() {
      this.makes = [];
      this.models = [];
      this.selectedMake = "";
      this.selectedModel = "";
      try {
        await axios.get(`https://rateengine.ship.cars/v2/vehicles/makes/?year=${this.selectedYear}&token=5cbe12fb62f4941267d623499a2a4fd5948fd3ef`,
            { headers: { Accept: "application/json", "Content-Type": "application/json" } }
        ).then(({data}) => {
          this.makes = data || [];
        });
      } catch (err) {
        this.error = "Failed to load makes. Please try again later.";
        console.error(err);
      }
    },
    async fetchModels() {
      this.models = [];
      this.selectedModel = "";
      try {
        await axios.get(`https://rateengine.ship.cars/v2/vehicles/models/?year=${this.selectedYear}&make=${this.selectedMake}&token=5cbe12fb62f4941267d623499a2a4fd5948fd3ef`,
            { headers: { Accept: "application/json", "Content-Type": "application/json" } }
        ).then(({data}) => {
          this.models = data || [];
        });
      } catch (err) {
        this.error = "Failed to load models. Please try again later.";
        console.error(err);
      }
    },
  },
  mounted() {
    this.fetchYears();
  },
};
</script>

<style scoped>
.vehicle-dropdown {
  display: flex;
  flex-direction: column;
  max-width: 400px;
  margin: auto;
}

label {
  margin-top: 1rem;
}

select {
  width: 100%;
  padding: 0.5rem;
  margin-top: 0.5rem;
}

.error {
  color: red;
  margin-top: 1rem;
}
</style>