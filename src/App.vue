<template>
  <div id="app">
    <Header />
    <div class="columns">
      <div class="column is-one-third-desktop">
        <GeocodingForm @geocode-success="updateResponse" />
      </div>
      <div class="column">
        <GeocodingResults :response="response" />
      </div>
    </div>
  </div>
</template>

<script>
import Header from './components/Header.vue';
import GeocodingForm from './components/GeocodingForm.vue';
import GeocodingResults from './components/GeocodingResults.vue';

export default {
  name: 'app',
  data() {
    return {
      response: {},
    };
  },
  components: {
    Header,
    GeocodingForm,
    GeocodingResults,
  },
  methods: {
    updateResponse(e) {
      this.response = e;
    },
  },
};
</script>

<style>
body {
  margin: 0;
  padding: 0;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', 'Oxygen',
    'Ubuntu', 'Cantarell', 'Fira Sans', 'Droid Sans', 'Helvetica Neue',
    sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

code {
  font-family: source-code-pro, Menlo, Monaco, Consolas, 'Courier New',
    monospace;
}
</style>
