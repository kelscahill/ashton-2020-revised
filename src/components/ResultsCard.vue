<template>
  <div class="c-results-card">
    <img src="../assets/logo.png" alt="2020 Revised" class="c-results-card__logo">
    <div class="c-results-card__body o-heading--l">Watch out world! This <div class="u-underline" v-html="results.value[0]"></div> superstar is ready to <div class="u-underline" v-html="results.value[1]"></div> and take 2021 by <div class="u-underline" v-html="results.value[2]"></div> storm. I’ll start by <div class="u-underline" v-html="results.value[3]"></div> and promise to always have <div class="u-underline" v-html="results.value[4]"></div> at the ready.  Here’s to a happier new year!</div>
    <div class="c-results-card__website">2020Revised.com</div>
  </div>
</template>

<script>
export default {
  name: "ResultsCard",
  props: ["results"]
}
</script>