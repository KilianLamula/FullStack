<template>
    <div class="home">
        <h2>Ville(s) pour un pays</h2>
        <label for="country">Choix du pays :</label>
        <div id="select">
            <select @change="afficherVilles($event)">
                <option v-for="country in data.countries" :key="country.id" :value="country._links.cities.href">
                    {{ country.name }}
                </option>
            </select>
        </div>
        <hr>
        <div class="tab">
            <h3>Ville(s) :</h3>
            <table>
                <thead>
                    <tr>
                        <th>Nom</th>
                        <th>Population</th>
                    </tr>
                </thead>
                <tbody v-for="city in data.cities" :key="city.id">
                    <tr>
                        <td>{{city.name}}</td>
                        <td>{{city.population}}</td>
                    </tr>
                </tbody>
            </table>
        </div>
    </div>

</template>

<script setup>
import { reactive, onMounted } from "vue";

let data = reactive({
  countries: [],
  cities: []
});

defineExpose({
  Pays,
})

function Pays(){
    fetch("api/countries")
    .then((response) => {return response.json()})
    .then((json) => {
      data.countries = json._embedded.countries;
    })
    .catch((error) => { // gestion des erreurs
        console.log(error)
    })
}

function afficherVilles(lien) {
    fetch(lien.target.value)
    .then((response) => {return response.json()})
    .then((json) => {
        data.cities = json._embedded.cities;
    })
    .catch((error) => { // gestion des erreurs
        console.log(error)
    })
}

onMounted(Pays);

</script>

<style>
thead {
    background-color: #333;
    color: #fff;
}

table {
  table-layout: fixed;
  width: 100%;
  border-collapse: collapse;
  border: 3px solid black;
}

th, td {
  padding: 20px;
}
</style>