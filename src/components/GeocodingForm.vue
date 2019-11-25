<template>
  <div class="box form">
    <form v-on:submit.prevent>
      <!-- API KEY -->
      <div class="field">
        <label class="label">API key</label>
        <div class="control has-icons-left">
          <span class="icon is-small is-left">
            <i class="fas fa-lock" />
          </span>
          <input v-model="apiKey" type="text" class="input" placeholder="YOUR-API-KEY" />
        </div>
        <div class="help">
          Your OpenCage Geocoder API Key (
          <a href="https://opencagedata.com/users/sign_up">register</a>
          ).
        </div>
      </div>
      <!-- ./API KEY -->

      <!-- Query -->
      <div class="field">
        <label class="label">Address or Coordinates</label>
        <div class="control has-icons-left">
          <span class="icon is-small is-left">
            <i class="fas fa-map-marked-alt" />
          </span>
          <input v-model="query" type="text" class="input" placeholder="location" />
          <div class="help">
            Address, place name
            <br />Coordinates as
            <code>latitude, longitude</code>
            or
            <code>y, x</code>.
          </div>
        </div>
      </div>
      <!-- ./Query -->

      <div class="field">
        <label class="label">Show my location</label>
        <div class="control">
          <button
            class="button is-light"
            v-bind:class="{'is-loading': isLocating}"
            v-on:click="handleGeoLocation"
          >
            <span class="icon is-small">
              <i class="fas fa-location-arrow" />
            </span>
          </button>
        </div>
      </div>

      <!-- Button Geocode -->
      <button
        class="button is-success"
        v-bind:disabled="isLocating || isSubmitting"
        v-on:click="geocode"
      >Geocode</button>
      <!-- ./Button Geocode -->
    </form>
  </div>
</template>

<script>
import * as opencage from 'opencage-api-client';

export default {
  name: 'form',
  data() {
    return {
      apiKey: null,
      query: null,
      isLocating: false,
      isSubmitting: false,
    };
  },
  methods: {
    handleGeoLocation() {
      const geolocation = navigator.geolocation;
      const p = new Promise((resolve, reject) => {
        if (!geolocation) {
          reject(new Error('Not Supported'));
        }
        this.isLocating = true;

        geolocation.getCurrentPosition(
          position => {
            console.log('Location found');
            resolve(position);
          },
          () => {
            console.log('Location : Permission denied');
            reject(new Error('Location : Permission denied'));
          }
        );
      });
      p.then(location => {
        const locationFound =
          location.coords.latitude + ', ' + location.coords.longitude;
        console.log(locationFound);
        this.query = locationFound;
        this.isLocating = false;
        this.$emit('geolocation-found', locationFound);
      });
    },
    geocode() {
      this.isSubmitting = true;
      console.log(this.apiKey);
      console.log(this.query);
      opencage
        .geocode({ key: this.apiKey, q: this.query })
        .then(response => {
          console.log(response);
          this.$emit('geocode-success', response);
          this.isSubmitting = false;
        })
        .catch(err => {
          console.error(err);
          this.isSubmitting = false;
        });
    },
  },
};
</script>

<style scoped>
.form {
  min-height: 50vh;
  display: block;
  margin: 15px 0px 15px 10px;
}
</style>