<template>
  <div class="l-wrap" role="document">
    <main class="l-main">
      <div class="l-main--inner">

        <article
          v-if="isHidden"
          class="c-panel c-panel--home"
        >
          <header class="c-panel__hero">
            <Logo />
          </header>
          <div class="c-panel__content">
            <div class="c-panel__content-body u-spacing animate__animated animate__fadeIn animate__slower">
              <div class="o-heading--m">202<span class="u-xout">0</span> should be left on the cutting room floor. Still, there are a few bright bits worth saving. Some quick <div class="u-underline"><span>revisions</span></div> could make a world of difference for the year ahead.</div>
            </div>
            <footer class="c-panel__content-footer">
              <button v-on:click="isHidden = false" class="o-button--primary animate__animated animate__fade-in-up animate__delay-1s">Start revising!</button>
              <Branding />
            </footer>
          </div>
        </article>

        <article
          v-if="!isHidden && resolutionIndex < quiz.resolutions.length"
          class="c-panel c-panel--resolutions"
          :class="{'is-revised':isRevised}"
        >
          <header class="c-panel__hero">
            <div class="o-progress">
              <span>{{ resolutionIndex + 1 }}/{{ quiz.resolutions.length }}</span>
              <progress :value="(resolutionIndex + 1) * 20" max="100"></progress>
            </div>
            <div class="c-panel__hero-heading">
              <h2 class="o-heading--xl">
                <span class="o-statement" v-html="quiz.resolutions[resolutionIndex].statement"></span>{{' '}}
                <span class="o-habit animate__animated" v-if="!isRevised" v-html="quiz.resolutions[resolutionIndex].habit"></span>
                <span class="o-habit animate__animated is-revised" v-if="isRevised" v-html="results.button[resolutionIndex]"></span>.
              </h2>
            </div>
          </header>
          <div class="c-panel__content">
            <div class="c-panel__content-body">
              <div class="c-panel__options u-spacing--half" v-if="!isRevised">
                <button
                  class="o-button--secondary animate__animated animate__fade-in-up"
                  v-bind:key="revision.id"
                  v-for="(revision, index) in quiz.resolutions[resolutionIndex].revisions"
                  v-on:click="revise(index, resolutionIndex)"
                  :id="'o-revision--' + resolutionIndex + '-' + index"
                  :data-button="revision.button"
                  :data-value="revision.value"
                  :data-gif="revision.gif"
                  v-html="revision.button"
                ></button>
              </div>
              <div class="c-panel__gif u-spacing animate__animated animate__fadeIn" v-if="isRevised">
                <img :src="results.gif[resolutionIndex]" alt="Gif" />
              </div>
            </div>
            <footer class="c-panel__content-footer">
              <button
                class="o-button--secondary animate__animated animate__fade-in-up"
                v-if="isRevised"
                v-on:click="next"
              >{{ quiz.resolutions[resolutionIndex].button_text }}</button>
              <Branding />
            </footer>
          </div>
        </article>

        <article
          v-if="resolutionIndex >= quiz.resolutions.length && resultIsHidden"
          class="c-panel c-panel--complete"
        >
          <header class="c-panel__hero">
            <h2 class="o-heading--xl">You did it!</h2>
            <img src="./assets/swivel.svg" alt="Swivel" />
          </header>
          <div class="c-panel__content">
            <div class="c-panel__content-body">
              <h3 class="o-heading--m">All of us at Ashton Design hope your new year revisions bring you a happy and healthier 2021.</h3>
            </div>
            <footer class="c-panel__content-footer">
              <button class="o-button--tertiary" v-on:click="resultIsHidden = !resultIsHidden">Final approval</button>
              <Branding />
            </footer>
          </div>
        </article>

        <article
          v-if="resolutionIndex >= quiz.resolutions.length && !resultIsHidden"
          class="c-panel c-panel--results"
        >
          <header class="c-panel__hero">
            <div id="card-square">
              <ResultsCard :results="results" />
            </div>
            <ResultsCard :results="results" />
          </header>
          <div class="c-panel__content">
            <footer class="c-panel__content-footer">
              <div class="c-social-share" v-if="!shareIsHidden">
                <a href="">Facebook</a>
                <a href="">Twitter</a>
                <a href="">Email</a>
                <a href="">SMS</a>
                <a id="download-square" download="2020-revised.jpg">Download Image</a>
              </div>
              <button class="o-button" v-on:click="screenshot(); shareIsHidden = !shareIsHidden">Share Your Revisions!</button>
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
          gif: "https://media.giphy.com/media/slOhiKAVFgwr6/source.gif",
          value: "<span>reconnecting</span><span>with family</span>"
        },
        {
          id: 2,
          button: "<span>exercising</span>",
          gif: "https://media.giphy.com/media/slOhiKAVFgwr6/source.gif",
          value: "<span>exercising</span>"
        },
        {
          id: 3,
          button: "<span>outside</span>",
          gif: "https://media.giphy.com/media/slOhiKAVFgwr6/source.gif",
          value: "<span>outside</span>"
        },
        {
          id: 4,
          button: "<span>drinking</span><span>better wine</span>",
          gif: "https://media.giphy.com/media/slOhiKAVFgwr6/source.gif",
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
          gif: "https://media.giphy.com/media/slOhiKAVFgwr6/source.gif",
          value: "<span>read a</span><span>book</span>"
        },
        {
          id: 2,
          button: "<span>drink a cup</span><span>of tea</span>",
          gif: "https://media.giphy.com/media/slOhiKAVFgwr6/source.gif",
          value: "<span>drink a cup</span><span>of tea</span>"
        },
        {
          id: 3,
          button: "<span>turn off</span><span>my phone</span>",
          gif: "https://media.giphy.com/media/slOhiKAVFgwr6/source.gif",
          value: "<span>turn off</span><span>my phone</span>"
        },
        {
          id: 4,
          button: "<span>practice my</span><span>dance moves</span>",
          gif: "https://media.giphy.com/media/slOhiKAVFgwr6/source.gif",
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
          gif: "https://media.giphy.com/media/slOhiKAVFgwr6/source.gif",
          value: "<span>relaxed</span>"
        },
        {
          id: 2,
          button: "<span>lively</span>",
          gif: "https://media.giphy.com/media/slOhiKAVFgwr6/source.gif",
          value: "<span>lively</span>"
        },
        {
          id: 3,
          button: "<span>intelligent</span>",
          gif: "https://media.giphy.com/media/slOhiKAVFgwr6/source.gif",
          value: "<span>intelligent</span>"
        },
        {
          id: 4,
          button: "<span>healthy</span>",
          gif: "https://media.giphy.com/media/slOhiKAVFgwr6/source.gif",
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
          gif: "https://media.giphy.com/media/slOhiKAVFgwr6/source.gif",
          value: "<span>making my bed</span>"
        },
        {
          id: 2,
          button: "<span>meditating</span><span>more</span>",
          gif: "https://media.giphy.com/media/slOhiKAVFgwr6/source.gif",
          value: "<span>meditating</span><span>more</span>"
        },
        {
          id: 3,
          button: "<span>showering</span><span>regularly</span>",
          gif: "https://media.giphy.com/media/slOhiKAVFgwr6/source.gif",
          value: "<span>showering</span><span>regularly</span>"
        },
        {
          id: 4,
          button: "<span>reading my</span><span>horoscope</span>",
          gif: "https://media.giphy.com/media/slOhiKAVFgwr6/source.gif",
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
          gif: "https://media.giphy.com/media/slOhiKAVFgwr6/source.gif",
          value: "<span>self-love</span>"
        },
        {
          id: 2,
          button: "<span>funny memes</span>",
          gif: "https://media.giphy.com/media/slOhiKAVFgwr6/source.gif",
          value: "<span>funny memes</span>"
        },
        {
          id: 3,
          button: "<span>empathy</span>",
          gif: "https://media.giphy.com/media/slOhiKAVFgwr6/source.gif",
          value: "<span>empathy</span>"
        },
        {
          id: 4,
          button: "<span>phone calls</span><span>to friends</span>",
          gif: "https://media.giphy.com/media/slOhiKAVFgwr6/source.gif",
          value: "<span>phone calls</span><span>to friends</span>"
        }
      ]
    }
  ]
};

import Logo from './components/Logo.vue'
import Branding from './components/Branding.vue'
import ResultsCard from './components/ResultsCard.vue'
import html2canvas from 'html2canvas'

export default {
  name: "App",
  components: {
    Logo,
    Branding,
    ResultsCard,
  },
  data() {
    return {
      quiz: quiz,
      resolutionIndex: 0,
      shareIsHidden: true,
      resultIsHidden: true,
      isHidden: true,
      isRevised: false,
      results: {
        button: [],
        value: [],
        gif: []
      }
    }
  },
  methods: {
    next() {
      if (this.resolutionIndex < this.quiz.resolutions.length) {
        this.resolutionIndex++;
      }
      this.isRevised = false;
    },
    revise(index, resolutionIndex) {
      var id = resolutionIndex + '-' + index;
      this.isRevised = true;
      this.results.button[resolutionIndex] = document.getElementById("o-revision--" + id).dataset.button;
      this.results.value[resolutionIndex] = document.getElementById("o-revision--" + id).dataset.value;
      this.results.gif[resolutionIndex] = document.getElementById("o-revision--" + id).dataset.gif;
    },
    screenshot() {
      var cardSquare = document.getElementById("card-square");
      html2canvas(cardSquare, {
        width: 600,
        height: 600
      }).then(function(canvas) {
        var image = canvas.toDataURL("image/jpg");
        var downloadSquareButton = document.getElementById("download-square");
        if (image && downloadSquareButton.href === "") {
          downloadSquareButton.href = image;
        }
      });
    },
  }
}
</script>