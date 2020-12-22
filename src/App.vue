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
              <button v-on:click="isHidden = false" class="o-button--primary animate__animated animate__fade-in-up animate__delay-1s">Let's go!</button>
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
              <h2 class="o-heading--xl" v-if="!isRevised">
                <span class="o-statement animate__animated animate__fadeIn" v-html="quiz.resolutions[resolutionIndex].statement"></span>{{' '}}
                <span class="o-habit u-strike" v-html="quiz.resolutions[resolutionIndex].habit"></span>
              </h2>
              <h2 class="o-heading--xl" v-if="isRevised">
                <span class="o-statement" v-html="quiz.resolutions[resolutionIndex].statement"></span>{{' '}}
                <span class="o-habit u-underline" v-html="results.button[resolutionIndex]"></span>
              </h2>
            </div>
          </header>
          <div class="c-panel__content">
            <div class="c-panel__content-body">
              <div class="c-panel__options u-spacing--half u-animation__delay" v-if="!isRevised">
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
                <img :src="require('./assets/gifs/' + resolutionIndex + '/' + selectedIndex + '.gif')" alt="Gif" />
              </div>
            </div>
            <footer class="c-panel__content-footer">
              <button
                class="o-button--secondary animate__animated animate__fade-in-up animate__delay-1s"
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
            <h2 class="o-heading--xl">Your future<br/>looks bright.</h2>
            <Scribble />
          </header>
          <div class="c-panel__content">
            <div class="c-panel__content-body">
              <h3 class="o-heading--m">
                Way to take things from bad to good!<br/><br/>We're making your revisions now. While we compute, Team Ashton would like to wish you a happier, healthier year ahead.
              </h3>
            </div>
            <footer class="c-panel__content-footer">
              <button class="o-button--tertiary" v-on:click="resultIsHidden = !resultIsHidden">See your masterpiece</button>
              <Branding />
            </footer>
          </div>
        </article>

        <article
          v-if="resolutionIndex >= quiz.resolutions.length && !resultIsHidden"
          class="c-panel c-panel--results"
          v-on:click="shareIsHidden = !shareIsHidden"
        >
          <header class="c-panel__hero">
            <div id="card-square">
              <ResultsCard :results="results" />
            </div>
            <ResultsCard :results="results" />
          </header>
          <div class="c-panel__content">
            <footer class="c-panel__content-footer">
              <div :class="'c-social-share animate__animated ' + [shareIsHidden ? 'animate__slideOutDown' : 'animate__slideInUp']">
                <a href="">
                  <span class="o-icon"><img src="./assets/icon-facebook.png" alt="Facebook" /></span>Facebook
                </a>
                <a href="">
                  <span class="o-icon"><img src="./assets/icon-twitter.png" alt="Twitter" /></span>Twitter
                </a>
                <a href="">
                  <span class="o-icon"><img src="./assets/icon-sms.png" alt="Message" /></span>Message
                </a>
                <a id="download-square" download="2020-revised.jpg">
                  <span class="o-icon"><img src="./assets/icon-instagram.png" alt="Instagram" /></span>Instagram<span class="o-small">(Download)</span>
                </a>
              </div>
              <button class="o-button" v-on:click="screenshot()">Share Your 2021 Goals</button>
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
      id: 0,
      statement: "This year I will spend more time",
      habit: "<span>in quarantine</span>",
      button_text: "Noice. Next!",
      revisions: [
        {
          id: 0,
          button: "<span>connecting</span><span>with family</span>",
          value: "<span>family-focused</span>"
        },
        {
          id: 1,
          button: "<span>exercising</span>",
          value: "<span>future-fit</span>"
        },
        {
          id: 2,
          button: "<span>outside</span>",
          value: "<span>“one with nature”</span>"
        },
        {
          id: 3,
          button: "<span>drinking</span><span>better wine</span>",
          value: "<span>fine-wine-drinking</span>"
        }
      ]
    },
    {
      id: 1,
      statement: "Every night before bed I will",
      habit: "<span>binge Netflix</span>",
      button_text: "We feel that. Onward!",
      revisions: [
        {
          id: 1,
          button: "<span>read a book</span>",
          value: "<span>become a bookworm</span>"
        },
        {
          id: 2,
          button: "<span>drink a cup</span><span>of tea</span>",
          value: "<span>sip tea</span>"
        },
        {
          id: 3,
          button: "<span>turn off</span><span>my phone</span>",
          value: "<span>unplug</span>"
        },
        {
          id: 4,
          button: "<span>practice my</span><span>dance moves</span>",
          value: "<span>become Tik Tok</span><span>famous</span>"
        }
      ]
    },
    {
      id: 2,
      statement: "I like to think of myself as",
      habit: "<span>contagious</span>",
      button_text: "Us too. Next!",
      revisions: [
        {
          id: 1,
          button: "<span>relaxed</span>",
          value: "<span>laid-back</span>"
        },
        {
          id: 2,
          button: "<span>lively</span>",
          value: "<span>fun-loving</span>"
        },
        {
          id: 3,
          button: "<span>intelligent</span>",
          value: "<span>whip-smart</span>"
        },
        {
          id: 4,
          button: "<span>healthy</span>",
          value: "<span>Covid-free</span>"
        }
      ]
    },
    {
      id: 3,
      statement: "One habit I’ll stick to is",
      habit: "<span>wearing</span><span>sweats 24/7</span>",
      button_text: "Just one left!",
      revisions: [
        {
          id: 1,
          button: "<span>making</span><span>my bed</span>",
          value: "<span>making</span><span>my bed</span>"
        },
        {
          id: 2,
          button: "<span>shopping</span><span>locally</span>",
          value: "<span>shopping</span><span>locally</span>"
        },
        {
          id: 3,
          button: "<span>showering</span><span>regularly</span>",
          value: "<span>showering</span><span>regularly</span>"
        },
        {
          id: 4,
          button: "<span>reading my</span><span>horoscope</span>",
          value: "<span>reading my</span><span>horoscope</span>"
        }
      ]
    },
    {
      id: 4,
      statement: "And I’ll try to never run short on",
      habit: "<span>toilet paper</span>",
      button_text: "Wrap it up!",
      revisions: [
        {
          id: 1,
          button: "<span>self-love</span>",
          value: "<span>self-love</span>"
        },
        {
          id: 2,
          button: "<span>funny memes</span>",
          value: "<span>funny memes</span>"
        },
        {
          id: 3,
          button: "<span>empathy</span>",
          value: "<span>empathy</span>"
        },
        {
          id: 4,
          button: "<span>USPS stamps</span>",
          value: "<span>USPS stamps</span>"
        }
      ]
    }
  ]
};

import Logo from './components/Logo.vue'
import Branding from './components/Branding.vue'
import Scribble from './components/Scribble.vue'
import ResultsCard from './components/ResultsCard.vue'
import html2canvas from 'html2canvas'

export default {
  name: "App",
  components: {
    Logo,
    Branding,
    Scribble,
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
      this.selectedIndex = index;
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