<template>
  <div class="box results">
    <div class="tabs is-boxed vh">
      <li class="is-active">
        <a>Results</a>
      </li>
    </div>
    <article class="message" v-if="response.hasOwnProperty('rate')">
      <div class="message-body">
        <p>Remaining {{response.rate.remaining}} out of {{response.rate.limit}} requests</p>
        <p>&nbsp;</p>
        <ol>
          <li v-for="(result,index) in response.results" v-bind:key="index">
            {{result.annotations.flag}} {{result.formatted}}
            <br />
            <code>{{result.geometry.lat}} {{result.geometry.lng}}</code>
          </li>
        </ol>
      </div>
    </article>
  </div>
</template>
<script>
export default {
  props: ['response'],
};
</script>
<style scoped>
.results {
  min-height: 50vh;
  display: block;
  margin: 15px 15px 10px 0px;
}
</style>