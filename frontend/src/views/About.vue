<template>
  <div class="about">
    <h1>Outils de développement</h1>
    <a href="http://localhost:8989/swagger-ui/" target="_blank">Swagger UI</a><br/>
    <a href="http://localhost:8989/h2-console/" target="_blank">console H2</a><br/>
    <a href="/api" target="_blank">api</a><br/>
    <country-list/>
  </div>
</template>

<script>
// @ is an alias to /src
import CountryList from '../components/CountryList.vue';

export default {
  name: "Home",
  components: {
    CountryList,
  },
};
</script>

