<template>
  <div class="l-wrap" role="document">
    <main class="l-main">
      <div class="l-main--inner">

        <article v-if="isHidden" class="c-panel c-panel__home">
          <header class="c-panel__hero">
            <img src="./assets/logo.svg" alt="2020 Revised">
          </header>
          <div class="c-panel__content">
            <p class="o-heading--m">2020 should be left on the cutting room floor. Still, there are a few bright bits worth saving. Some quick revisions could make a world of difference for the year ahead.</p>
          </div>
          <footer>
            <button v-on:click="isHidden = false" class="o-button">Start revising!</button>
          </footer>
        </article>

        <article v-if="!isHidden && resolutionIndex < quiz.resolutions.length" v-bind:key="resolutionIndex" class="c-panel c-panel__resolutions">
          <header class="c-panel__hero">
            <div class="o-counter">{{ resolutionIndex + 1 }}/{{ quiz.resolutions.length }}</div>
            <h2 class="o-heading--l">{{ quiz.resolutions[resolutionIndex].statement }} <span class="u-strikethrough">{{ quiz.resolutions[resolutionIndex].habit }}</span>.</h2>
          </header>
          <div class="c-panel__content">
            <div class="c-panel__options" v-if="!isRevised">
              <button
              :id="'option-' + resolutionIndex + '-' + revision.id"
              class="o-button--tertiary"
              :class="{'is-active':isSelected == index + 1}"
              v-bind:key="revision.id"
              v-for="(revision, index) in quiz.resolutions[resolutionIndex].revisions"
              @click="isSelected = index + 1"
              >{{ revision.button }}</button>
            </div>
            <div class="c-panel__description" v-if="isRevised">
              <div
              :class="'option-' + resolutionIndex + '-' + revision.id"

              v-bind:key="revision.id"
              v-for="revision in quiz.resolutions[resolutionIndex].revisions"
              v-html="revision.description"></div>
            </div>
          </div>
          <footer>
            <button class="o-button--secondary" v-if="!isRevised" v-on:click="isRevised = true" :disabled="isSelected <= 0">Revise it!</button>
            <button class="o-button--secondary" v-on:click="next" v-if="isRevised">Next</button>
            <div class="o-branding">Created by <img src="./assets/logo-ashton.svg" alt="Ashton Design" /></div>
          </footer>
        </article>

        <article v-if="resolutionIndex >= quiz.resolutions.length" v-bind:key="resolutionIndex" class="c-panel c-panel__results">
          <header class="c-panel__hero">
            <div class="c-card">
              <img src="./assets/logo.svg" alt="2020 Revised">
              <p>Watch out world! This ____ superstar is ready to ____ and take 2021 by ____ storm. I’ll start by ____ and promise to always have ____ at the ready.  Here’s to a happier new year!</p>
            </div>
          </header>
          <div class="c-panel__content">
            <h3 class="o-heading--m">All of us at Ashton Design hope your new year revisions bring you a happy and healthier 2021.</h3>
          </div>
          <footer>
            <div class="c-social-share">
              <a href="">Facebook</a>
              <a href="">Twitter</a>
              <a href="">Email</a>
              <a href="">SMS</a>
            </div>
            <button class="o-button">Share Your Revisions!</button>
            <div class="o-branding">Created by <img src="./assets/logo-ashton.svg" alt="Ashton Design" /></div>
          </footer>
        </article>

      </div>
    </main>
  </div>
</template>

<script>
var quiz = {
  resolutions: [
    {
      id: 1,
      statement: "This year I will spend more time",
      habit: "quarantining",
      button_text: "Next",
      revisions: [
        {
          id: 1,
          button: "Reconnecting with family",
          description: "<h4>Title</h4><p>Read, lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.</p>",
          value: "Reconnecting with family"
        },
        {
          id: 2,
          button: "Exercising",
          description: "Meditate, lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.",
          value: "Exercising"
        },
        {
          id: 3,
          button: "Outside",
          description: "Drink, lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.",
          value: "Outside"
        },
        {
          id: 4,
          button: "Drinking better wine",
          description: "Stretch, lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.",
          value: "Drinking better wine"
        }
      ]
    },
    {
      id: 2,
      statement: "Once a week I will",
      habit: "finish a bottle of wine",
      button_text: "Keep going",
      revisions: [
        {
          id: 1,
          button: "call my parents",
          description: "Call, lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.",
          value: "call my parents"
        },
        {
          id: 2,
          button: "run 10 miles",
          description: "Run, lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.",
          value: "run 10 miles"
        },
        {
          id: 3,
          button: "clean my room",
          description: "Clean, lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.",
          value: "clean my room"
        },
        {
          id: 4,
          button: "turn off my phone",
          description: "Turn off my phone, lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.",
          value: "turn off my phone"
        }
      ]
    }
  ]
};

export default {
  name: "App",
  data() {
    return {
      quiz: quiz,
      resolutionIndex: 0,
      isHidden: true,
      isRevised: false,
      isSelected: 0
    }
  },
  computed: {
    disabled() {
      return this.selected.length < 1; // or === 0
    }
  },
  methods: {
    restart() {
      this.resolutionIndex = 0;
    },
    next() {
      if (this.resolutionIndex < this.quiz.resolutions.length) {
        this.resolutionIndex++;
      }
      this.isSelected = 0;
      this.isRevised = false;
    },
    revise() {

    }
  }
}
</script>