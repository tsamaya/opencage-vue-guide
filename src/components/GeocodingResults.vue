<template>
  <div class="box results">
    <div class="tabs is-boxed vh">
      <ul>
        <li v-bind:class="{ 'is-active': activTab === RESULT_TAB }">
          <a href="#" v-on:click.prevent="setActiveTab(RESULT_TAB)">
            <span class="icon is-small">
              <i class="fas fa-list-ul" aria-hidden="true" />
            </span>
            <span>Results</span>
          </a>
        </li>
        <li
          v-bind:class="[
            { 'is-active': activTab === MAP_TAB },
            { 'is-hidden': !response.hasOwnProperty('rate') },
          ]"
        >
          <a href="#" v-on:click.prevent="setActiveTab(MAP_TAB)">
            <span class="icon is-small">
              <i class="fas fa-globe-americas" aria-hidden="true" />
            </span>
            <span>Map</span>
          </a>
        </li>
        <li
          v-bind:class="[
            { 'is-active': activTab === JSON_TAB },
            { 'is-hidden': !response.hasOwnProperty('rate') },
          ]"
        >
          <a href="#" v-on:click.prevent="setActiveTab(JSON_TAB)">
            <span class="icon is-small">
              <i class="fab fa-js" aria-hidden="true" />
            </span>
            <span>JSON Result</span>
          </a>
        </li>
      </ul>
    </div>
    <!-- Result sumary -->
    <article class="message" v-if="activTab === RESULT_TAB">
      <div class="message-body" v-if="response.hasOwnProperty('rate')">
        <p>
          Remaining {{ response.rate.remaining }} out of
          {{ response.rate.limit }} requests
        </p>
        <br />
        <ol>
          <li v-for="(result, index) in response.results" v-bind:key="index">
            {{ result.annotations.flag }} {{ result.formatted }}
            <br />
            <code>{{ result.geometry.lat }} {{ result.geometry.lng }}</code>
          </li>
        </ol>
      </div>
    </article>
    <!-- JSON response -->
    <article class="message" v-if="activTab === JSON_TAB">
      <div class="message-body" v-if="response.hasOwnProperty('rate')">
        <pre>{{ JSON.stringify(response, null, 2) }}</pre>
      </div>
    </article>
    <!-- MAP response -->
    <div id="map" v-bind:class="{ 'is-hidden': activTab !== MAP_TAB }"></div>
  </div>
</template>
<script>
import './../../node_modules/leaflet/dist/leaflet.css';
import L from 'leaflet';
const RESULT_TAB = 'RESULT_TAB';
const MAP_TAB = 'MAP_TAB';
const JSON_TAB = 'JSON_TAB';

const redIcon = L.icon({
  iconUrl: 'marker-icon-red.png',
  iconSize: [25, 41], // size of the icon
  iconAnchor: [12, 40], // point of the icon which will correspond to marker's location
});

export default {
  props: ['response'],
  data() {
    return {
      RESULT_TAB,
      MAP_TAB,
      JSON_TAB,
      activTab: RESULT_TAB,
      mapInit: false,
      layer: null,
      map: null,
    };
  },
  mounted() {
    this.initMap();
  },
  methods: {
    setActiveTab(tab) {
      this.activTab = tab;
    },
    initMap() {
      if (this.mapInit) return;

      // creates the Leaflet map object
      // it is called after the Map component mounts
      const map = L.map('map', {
        center: [45, 2],
        zoom: 4,
      });

      L.tileLayer(
        'https://cartodb-basemaps-{s}.global.ssl.fastly.net/rastertiles/voyager/{z}/{x}/{y}.png',
        {
          maxZoom: 18,
          attribution:
            '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>, &copy; <a href="https://carto.com/attribution">CARTO</a>',
        }
      ).addTo(map);
      // L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
      //   attribution:
      //     '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors',
      // }).addTo(map);

      const layer = L.featureGroup().addTo(map);

      // set the state
      this.mapInit = true;
      this.map = map;
      this.layer = layer;
    },
    updateMap() {
      if (!this.response) return;
      const { results } = this.response;
      if (!results) return;
      for (let i = 0; i < results.length; i++) {
        const marker = L.marker(results[i].geometry, { icon: redIcon });
        marker
          .addTo(this.layer)
          .bindPopup(i + 1 + ' - ' + results[i].formatted);
      }
      this.map.fitBounds(this.layer.getBounds());
    },
  },
};
</script>
<style scoped>
.results {
  min-height: 50vh;
  display: block;
  margin: 15px 15px 10px 0px;
}
pre {
  overflow: hidden;
  /* max-width: 600px; */
  /* text-overflow: ellipsis; */
  white-space: pre-wrap;
  word-wrap: break-word;
  overflow-wrap: break-word;
}
#map {
  width: auto;
  min-height: 350px;
  height: 40vmin;
}
</style>
