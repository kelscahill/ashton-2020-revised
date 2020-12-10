<template>
  <section class="o-panel o-panel__resolutions c-resolutions" v-bind:key="resolution.id" v-for="resolution in quiz.resolutions">
    <header></header>
    <div class="o-panel--top u-background-color--maroon u-color--pink">
      <h1>{{ resolution.resolution_statement }} <span class="u-strikethrough">{{ resolution.resolution_habit }}</span>.</h1>
    </div>
    <div class="o-panel--bottom">
      <div class="" v-bind:key="revision.id" v-for="revision in resolution.revisions">
        <button>{{ revision.button }}</button>
        {{ revision.text }}
      </div>
      <button v-on:click="next">{{ resolution.button_text }}</button>
    </div>
    <footer></footer>
  </section>
</template>

<script>
export default {
  name: "Resolutions",
  props: ["resolutions"]
}
</script>