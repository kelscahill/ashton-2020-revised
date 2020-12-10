<template>
<div class="o-resolutions__item">
  <p>{{ resolution.resolution_statement }}</p>
  <p>{{ resolution.resolution_habit }}</p>

  <div class="o-panel o-panel__resolutions c-resolutions" v-bind:key="revision.id" v-for="revision in resolution.revisions">
    {{ revision.button }}
  </div>

  <button v-on:click="next">{{ resolution.button_text }}</button>
</div>
</template>

<script>
export default {
  name: "ResolutionsItem",
  props: ["resolution"]
}
</script>