<template>
  <div class="box results">
    <div class="tabs is-boxed vh">
      <ul>
        <li v-bind:class="{'is-active': activTab === RESULT_TAB}">
          <a href="#" v-on:click.prevent="setActiveTab(RESULT_TAB)">
            <span class="icon is-small">
              <i class="fas fa-list-ul" aria-hidden="true" />
            </span>
            <span>Results</span>
          </a>
        </li>
        <li v-bind:class="{'is-active': activTab === MAP_TAB}">
          <a href="#" v-on:click.prevent="setActiveTab(MAP_TAB)">
            <span class="icon is-small">
              <i class="fas fa-globe-americas" aria-hidden="true" />
            </span>
            <span>Map</span>
          </a>
        </li>
        <li v-bind:class="{'is-active': activTab === JSON_TAB}">
          <a href="#" v-on:click.prevent="setActiveTab(JSON_TAB)">
            <span class="icon is-small">
              <i class="fas fa-js" aria-hidden="true" />
            </span>
            <span>JSON Result</span>
          </a>
        </li>
      </ul>
    </div>
    <!-- Result sumary -->
    <article class="message" v-if="activTab === RESULT_TAB">
      <div class="message-body" v-if="response.hasOwnProperty('rate')">
        <p>Remaining {{response.rate.remaining}} out of {{response.rate.limit}} requests</p>
        <br />
        <ol>
          <li v-for="(result,index) in response.results" v-bind:key="index">
            {{result.annotations.flag}} {{result.formatted}}
            <br />
            <code>{{result.geometry.lat}} {{result.geometry.lng}}</code>
          </li>
        </ol>
      </div>
    </article>
    <!-- JSON response -->
    <article class="message" v-if="activTab === MAP_TAB">
      <div class="message-body">
        <pre>Coming soon!</pre>
      </div>
    </article>
    <!-- JSON response -->
    <article class="message" v-if="activTab === JSON_TAB">
      <div class="message-body">
        <pre>{{JSON.stringify(response, null, 2)}}</pre>
      </div>
    </article>
  </div>
</template>
<script>
const RESULT_TAB = 'RESULT_TAB';
const MAP_TAB = 'MAP_TAB';
const JSON_TAB = 'JSON_TAB';

export default {
  props: ['response'],
  data() {
    return {
      RESULT_TAB,
      MAP_TAB,
      JSON_TAB,
      activTab: RESULT_TAB,
    };
  },
  methods: {
    setActiveTab(tab) {
      this.activTab = tab;
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
</style>