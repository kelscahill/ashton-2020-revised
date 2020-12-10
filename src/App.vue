<template>
  <div class="l-wrap" role="document">
    <main class="l-main">
      <div class="l-main--inner">

        <section v-if="isHidden" class="o-panel o-panel__home">
          <div class="o-panel--top u-background-color--maroon">
            <img src="./assets/logo.svg" alt="2020 Revised">
          </div>
          <div class="o-panel--bottom u-spacing">
            <p>2020 may not have been all that we hoped for, so lets make some revisions for the coming year.</p>
            <button v-on:click="isHidden = false" class="o-button">Start revising!</button>
          </div>
        </section>

        <section v-if="!isHidden">
          <div class="c-resolutions" v-if="resolutionIndex < quiz.resolutions.length" v-bind:key="resolutionIndex">
            <section class="o-panel o-panel__resolutions">
              <header>Progress Bar</header>
              <div class="o-panel--top u-background-color--maroon u-color--pink">
                <h1>{{ quiz.resolutions[resolutionIndex].statement }} <span class="u-strikethrough">{{ quiz.resolutions[resolutionIndex].habit }}</span>.</h1>
              </div>
              <div class="o-panel--bottom" v-bind:key="revision.id" v-for="revision in quiz.resolutions[resolutionIndex].revisions">
                <button>{{ revision.button }}</button>
                <div class="">

                  <button v-on:click="next">Next</button>
                </div>
                <footer>Ashton</footer>
              </div>
            </section>
          </div>

          <div v-else-if="resolutionIndex >= quiz.resolutions.length" v-bind:key="resolutionIndex">
            <p>Completed</p>
          </div>
        </section>

      </div>
    </main>
  </div>
</template>

<script>
var quiz = {
  resolutions: [
    {
      id: 1,
      statement: "Every night before bed I will",
      habit: "binge Netflix",
      button_text: "Next",
      revisions: [
        {
          id: 1,
          button: "read a book",
          description: "Read, lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.",
          value: "read a book"
        },
        {
          id: 2,
          button: "meditate",
          description: "Meditate, lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.",
          value: "meditate"
        },
        {
          id: 3,
          button: "drink a cup of tea",
          description: "Drink, lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.",
          value: "drink a cup of tea"
        },
        {
          id: 4,
          button: "stretch",
          description: "Stretch, lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.",
          value: "stretch"
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
var userResponseSkelaton = Array(quiz.resolutions.length).fill(null);

export default {
  name: "App",
  data() {
    return {
      quiz: quiz,
      resolutionIndex: 0,
      userResponses: userResponseSkelaton,
      isHidden: true
    }
  },
  methods: {
    restart() {
      this.resolutionIndex = 0;
      this.userResponses = Array(this.quiz.resolutions.length).fill(null);
    },
    next() {
      if (this.resolutionIndex < this.quiz.resolutions.length) {
        this.resolutionIndex++;
      }
    }
  }
}
</script>