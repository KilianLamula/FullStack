<template>
  <div class="home">
    <h2>Edition des villes</h2>
    <div class="row">
      <div class="col">
        <form @submit.prevent="saveCity">
          <div class="form-group">
            <label for="country">Pays:</label>
            <select class="form-control" v-model="data.editedCity.country">
              <option disabled value="0">Choisissez un pays</option>
              <option
                v-for="country in data.allCountries"
                :key="country.id"
                :value="country.id"
              >
                {{ country.name }}
              </option>
            </select>
          </div>
          <div class="mb-3 mt-3">
            <label for="name" class="form-label">Nom:</label>
            <input class="form-control" v-model="data.editedCity.name" />
          </div>
          <div class="mb-3">
            <label for="population" class="form-label">Population:</label>
            <input
              class="form-control"
              v-model="data.editedCity.population"
              type="number"
            />
          </div>
          <button
            type="submit"
            class="btn btn-primary"
            :disabled="data.editedCity.country == 0"
          >
            Ajouter une ville
          </button>
        </form>
      </div>
      <div class="col">
        <city-list v-bind:cities="data.allCities" />
      </div>
    </div>
  </div>
</template>

<script setup>
import { reactive, onMounted } from "vue";
// @ is an alias to /src
import CityList from "@/components/CityList.vue";

const emptyCity = {
  id: "",
  name: "",
  population: 1,
  country: 0,
};

const data = reactive({
  allCities: [],
  allCountries: [],
  editedCity: { ...emptyCity },
});

function fetchCities() {
  fetch("api/cities")
    .then((response) => response.json())
    .then((json) => {
      data.allCities = json._embedded.cities;
    })
    .catch((error) => alert(error));
}

function fetchCountries() {
  fetch("api/countries")
    .then((response) => response.json())
    .then((json) => {
      data.allCountries = json._embedded.countries;
    })
    .catch((error) => alert(error));
}

function saveCity() {
  //var data = new URLSearchParams(this.editedCity).toString();
  ajaxSaveCity()
    .then((json) => {
      data.allCities.push(json);
      data.editedCity = { ...emptyCity };
    })
    .catch((error) => alert(error));
}

async function ajaxSaveCity() {
  const options = {
    method: "POST",
    mode: "cors",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(data.editedCity),
  };
  const response = await fetch("api/saveCity", options);
  if (!response.ok) {
    // status != 2XX
    const message = await response.text();
    throw new Error(message);
  }
  return response.json();
}

// Au chargement du composant
onMounted(() => {
  fetchCities(); // On récupère les villes (pour la table)
  fetchCountries(); // On récupère les pays (pour le sélecteur de pays)
});
</script>
