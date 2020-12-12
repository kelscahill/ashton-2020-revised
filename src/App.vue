<template>
  <div class="l-wrap" role="document">
    <main class="l-main">
      <div class="l-main--inner">

        <article
        v-if="isHidden"
        class="c-panel c-panel--home">
          <header class="c-panel__hero">
            <Logo />
          </header>
          <div class="c-panel__content">
            <div class="c-panel__content-body u-spacing">
              <Branding />
              <div class="o-heading--l">202<span class="u-xout">0</span> should be left on the cutting room floor. Still, there are a few bright bits worth saving. Some quick <div class="u-underline"><span>revisions</span></div> could make a world of difference for the year ahead.</div>
            </div>
            <footer class="c-panel__content-footer">
              <button v-on:click="isHidden = false" class="o-button--primary animate__animated animate__pulse animate__delay-3s">Start revising!</button>
            </footer>
          </div>
        </article>

        <article
        v-if="!isHidden && resolutionIndex < quiz.resolutions.length"
        class="c-panel c-panel--resolutions animate__animated animate__fadeIn"
        :class="{'is-revised':isRevised}">
          <header class="c-panel__hero">
            <div class="o-counter">{{ resolutionIndex + 1 }}/{{ quiz.resolutions.length }}</div>
            <h2 class="o-heading--xl">{{ quiz.resolutions[resolutionIndex].statement }} <div class="o-habit animate__animated animate__pulse" v-html="isRevised ? results.button[resolutionIndex] : quiz.resolutions[resolutionIndex].habit"></div>.</h2>
          </header>
          <div class="c-panel__content">
            <div class="c-panel__content-body">
              <div class="c-panel__options" v-if="!isRevised">
                <button
                class="o-button--tertiary"
                v-bind:key="revision.id"
                v-for="(revision, index) in quiz.resolutions[resolutionIndex].revisions"
                v-on:click="selected(index)"
                :class="{'is-active':isSelected == index + 1}"
                :data-button="revision.button"
                :data-value="revision.value"
                :data-description="revision.description"
                v-html="revision.button"
                ></button>
              </div>
              <div class="c-panel__description u-spacing animate__animated animate__zoomIn" v-if="isRevised" v-html="results.description[resolutionIndex]"></div>
            </div>
            <footer class="c-panel__content-footer">
              <button class="o-button--secondary" v-if="!isRevised" v-on:click="revise(resolutionIndex)" :disabled="isSelected <= 0">{{ isSelected ? 'Revise it!' : 'Choose a Revision above' }}</button>
              <button class="o-button--secondary" v-on:click="next" v-if="isRevised">{{ quiz.resolutions[resolutionIndex].button_text }}</button>
              <Branding />
            </footer>
          </div>
        </article>

        <article
        v-if="resolutionIndex >= quiz.resolutions.length"
        class="c-panel c-panel--results">
          <header class="c-panel__hero">
            <div class="c-results-card">
              <img src="./assets/logo.svg" alt="2020 Revised" class="c-results-card__logo">
              <div class="c-results-card__body o-heading--m">Watch out world! This <div class="u-strike" v-html="quiz.resolutions[0].habit"></div> <div class="u-underline" v-html="results.value[0]"></div> superstar is ready to <div class="u-strike" v-html="quiz.resolutions[1].habit"></div> <div class="u-underline" v-html="results.value[1]"></div> and take 2021 by <div class="u-strike" v-html="quiz.resolutions[2].habit"></div> <div class="u-underline" v-html="results.value[2]"></div> storm. I’ll start by <div class="u-strike" v-html="quiz.resolutions[3].habit"></div> <div class="u-underline" v-html="results.value[3]"></div> and promise to always have <div class="u-strike" v-html="quiz.resolutions[4].habit"></div> <div class="u-underline" v-html="results.value[4]"></div> at the ready.  Here’s to a happier new year!</div>
              <div class="c-results-card__website">2020Revised.com</div>
            </div>
          </header>
          <div class="c-panel__content">
            <div class="c-panel__content-body">
              <h3 class="o-heading--l">All of us at Ashton Design hope your new year revisions bring you a happy and healthier 2021.</h3>
            </div>
            <footer class="c-panel__content-footer">
              <div class="c-social-share">
                <a href="">Facebook</a>
                <a href="">Twitter</a>
                <a href="">Email</a>
                <a href="">SMS</a>
              </div>
              <button class="o-button">Share Your Revisions!</button>
              <Branding />
            </footer>
          </div>
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
      habit: "<span>quarantining</span>",
      button_text: "Next",
      revisions: [
        {
          id: 1,
          button: "<span>reconnecting</span><span>with family</span>",
          description: "<h4><span>reconnecting</span><span>with family</span></h4><p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.</p>",
          value: "<span>reconnecting</span><span>with family</span>"
        },
        {
          id: 2,
          button: "<span>exercising</span>",
          description: "<h4><span>exercising</span></h4><p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.</p>",
          value: "<span>exercising</span>"
        },
        {
          id: 3,
          button: "<span>outside</span>",
          description: "<h4><span>outside</span></h4><p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.</p>",
          value: "<span>outside</span>"
        },
        {
          id: 4,
          button: "<span>drinking</span><span>better wine</span>",
          description: "<h4><span>drinking</span><span>better wine</span></h4><p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.</p>",
          value: "<span>drinking</span><span>better wine</span>"
        }
      ]
    },
    {
      id: 2,
      statement: "Every night before bed I will",
      habit: "<span>binge</span><span>Netflix</span>",
      button_text: "Next",
      revisions: [
        {
          id: 1,
          button: "<span>read a</span><span>book</span>",
          description: "<h4><span>read a</span><span>book</span></h4><p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.</p>",
          value: "<span>read a</span><span>book</span>"
        },
        {
          id: 2,
          button: "<span>drink a cup</span><span>of tea</span>",
          description: "<h4><span>drink a cup</span><span>of tea</span></h4><p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.</p>",
          value: "<span>drink a cup</span><span>of tea</span>"
        },
        {
          id: 3,
          button: "<span>turn off</span><span>my phone</span>",
          description: "<h4><span>turn off</span><span>my phone</span></h4><p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.</p>",
          value: "<span>turn off</span><span>my phone</span>"
        },
        {
          id: 4,
          button: "<span>practice my</span><span>dance moves</span>",
          description: "<h4><span>practice my</span><span>dance moves</span></h4><p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.</p>",
          value: "<span>practice my</span><span>dance moves</span>"
        }
      ]
    },
    {
      id: 3,
      statement: "I like to think of myself as",
      habit: "<span>contagious</span>",
      button_text: "Next",
      revisions: [
        {
          id: 1,
          button: "<span>relaxed</span>",
          description: "<h4><span>relaxed</span></h4><p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.</p>",
          value: "<span>relaxed</span>"
        },
        {
          id: 2,
          button: "<span>lively</span>",
          description: "<h4><span>lively</span></h4><p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.</p>",
          value: "<span>lively</span>"
        },
        {
          id: 3,
          button: "<span>intelligent</span>",
          description: "<h4><span>intelligent</span></h4><p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.</p>",
          value: "<span>intelligent</span>"
        },
        {
          id: 4,
          button: "<span>healthy</span>",
          description: "<h4><span>healthy</span></h4><p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.</p>",
          value: "<span>healthy</span>"
        }
      ]
    },
    {
      id: 4,
      statement: "One habit I’ll keep is",
      habit: "<span>ordering</span><span>takeout</span>",
      button_text: "Next",
      revisions: [
        {
          id: 1,
          button: "<span>making my bed</span>",
          description: "<h4><span>making my bed</span></h4><p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.</p>",
          value: "<span>making my bed</span>"
        },
        {
          id: 2,
          button: "<span>meditating</span><span>more</span>",
          description: "<h4><span>meditating</span><span>more</span></h4><p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.</p>",
          value: "<span>meditating</span><span>more</span>"
        },
        {
          id: 3,
          button: "<span>showering</span><span>regularly</span>",
          description: "<h4><span>showering</span><span>regularly</span></h4><p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.</p>",
          value: "<span>showering</span><span>regularly</span>"
        },
        {
          id: 4,
          button: "<span>reading my</span><span>horoscope</span>",
          description: "<h4><span>reading my</span><span>horoscope</span></h4><p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.</p>",
          value: "<span>reading my</span><span>horoscope</span>"
        }
      ]
    },
    {
      id: 5,
      statement: "And I’ll try to never run short on",
      habit: "<span>toilet paper</span>",
      button_text: "Next",
      revisions: [
        {
          id: 1,
          button: "<span>self-love</span>",
          description: "<h4><span>self-love</span></h4><p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.</p>",
          value: "<span>self-love</span>"
        },
        {
          id: 2,
          button: "<span>funny memes</span>",
          description: "<h4>funny memes</h4><p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.</p>",
          value: "<span>funny memes</span>"
        },
        {
          id: 3,
          button: "<span>empathy</span>",
          description: "<h4><span>empathy</span></h4><p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.</p>",
          value: "<span>empathy</span>"
        },
        {
          id: 4,
          button: "<span>phone calls</span><span>to friends</span>",
          description: "<h4><span>phone calls</span><span>to friends</span></h4><p>Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.</p>",
          value: "<span>phone calls</span><span>to friends</span>"
        }
      ]
    }
  ]
};

import Logo from './components/Logo.vue'
import Branding from './components/Branding.vue'

export default {
  name: "App",
  components: {
    Logo,
    Branding
  },
  data() {
    return {
      quiz: quiz,
      resolutionIndex: 0,
      isHidden: true,
      isRevised: false,
      isSelected: 0,
      results: {
        button: [],
        value: [],
        description: []
      }
    }
  },
  methods: {
    next() {
      if (this.resolutionIndex < this.quiz.resolutions.length) {
        this.resolutionIndex++;
      }
      this.isSelected = 0;
      this.isRevised = false;
    },
    selected(index) {
      this.isSelected = index + 1;
    },
    revise(index) {
      this.isRevised = true;
      this.results.button[index] = document.querySelector('.o-button--tertiary.is-active').dataset.button;
      this.results.value[index] = document.querySelector('.o-button--tertiary.is-active').dataset.value;
      this.results.description[index] = document.querySelector('.o-button--tertiary.is-active').dataset.description;
    }
  }
}
</script>