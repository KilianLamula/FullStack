<template>
  <div>
    <table>
      <tr>
        <th>Code</th>
        <th>Nom</th>
      </tr>
      <tr v-for="country in countries" :key="country.id">
        <td>{{ country.code }}</td>
        <td>{{ country.name }}</td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  name: "CountryList",
  data() {
    return {
      countries: [],
    };
  },
  mounted() {
    fetch("api/countries")
      .then((response) => response.json())
      .then((json) => {
        console.log(json);
        this.countries = json._embedded.countries;
      })
      .catch((error) => alert(error));
  },
};
</script>

<style scoped></style>