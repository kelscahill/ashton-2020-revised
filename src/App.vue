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
              <div class="o-heading--m">202<span class="u-xout">0</span> should be left on the cutting room floor. Still, there are a few bits worth saving. Some quick <div class="u-underline"><span>revisions</span></div> could make a world of difference for the year ahead.</div>
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
                <span class="o-statement animate__animated animate__fadeIn" v-html="quiz.resolutions[resolutionIndex].statement"></span>{{' '}}
                <span class="o-habit u-underline" v-html="results.button[resolutionIndex]"></span>
              </h2>
            </div>
          </header>
          <div class="c-panel__content">
            <div class="c-panel__content-body">
              <div class="c-panel__options u-spacing--half u-animation__delay" v-if="!isRevised">
                <button
                  class="o-button--secondary animate__animated animate__fadeInUp"
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
                class="o-button--secondary animate__animated animate__fadeInUp animate__delay-1s"
                v-if="isRevised"
                v-on:click="next"
              >{{ quiz.resolutions[resolutionIndex].button_text }}</button>
            </footer>
          </div>
        </article>

        <article
          v-if="resolutionIndex >= quiz.resolutions.length && resultIsHidden"
          class="c-panel c-panel--complete"
        >
          <header class="c-panel__hero">
            <h2 class="o-heading--xl animate__animated animate__fade-in-up">Your future<br/>looks bright.</h2>
            <Scribble />
          </header>
          <div class="c-panel__content">
            <div class="c-panel__content-body animate__animated animate__fadeIn">
              <h3 class="o-heading--m">
                Way to take things from bad to good! We're making your revisions now.<br/><br/>In the meantime, Team Ashton would like to wish you a happier, healthier year ahead.
              </h3>
            </div>
            <footer class="c-panel__content-footer">
              <button class="o-button--tertiary animate__animated animate__fade-in-up" v-on:click="resultIsHidden = !resultIsHidden">See your masterpiece</button>
            </footer>
          </div>
        </article>

        <article
          v-if="resolutionIndex >= quiz.resolutions.length && !resultIsHidden"
          class="c-panel c-panel--results"
        >
          <header class="c-panel__hero" v-on:click="removeClass()">
            <div id="card-square">
              <ResultsCard :results="results" />
            </div>
            <ResultsCard :results="results" class="animate__animated animate__fadeIn" />
          </header>
          <div class="c-panel__content">
            <footer class="c-panel__content-footer">
              <div id="social-share" class="c-social-share">
                <a target="_blank" :href="'https://facebook.com/sharer/sharer.php?u=' + pageUrl">
                  <span class="o-icon"><img src="./assets/icon-facebook.png" alt="Facebook" /></span>Facebook
                </a>
                <a target="_blank" :href="'https://twitter.com/intent/tweet/?text=' + pageTitle + ' @ashtondesignllc&amp;url=' + pageUrl">
                  <span class="o-icon"><img src="./assets/icon-twitter.png" alt="Twitter" /></span>Twitter
                </a>
                <a target="_blank" :href="'https://www.addtoany.com/add_to/sms?linkurl=' + pageUrl + '&linkname=' + pageTitle">
                  <span class="o-icon"><img src="./assets/icon-sms.png" alt="Message" /></span>Message
                </a>
                <a v-on:click="screenshot()" class="o-button__screenshot">
                  <span class="o-icon"><img src="./assets/icon-instagram.png" alt="Instagram" /></span>
                  <font>Instagram<br /><span class="o-small">(Download Your Results)</span></font>
                </a>
              </div>
              <button class="o-button animate__animated animate__fade-in-up" v-on:click="addClass()">Share with your friends</button>
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
      statement: "Every night before bed, I will",
      habit: "<span>binge Netflix</span>",
      button_text: "We feel that. Onward!",
      revisions: [
        {
          id: 1,
          button: "<span>read a book</span>",
          value: "<span>become a</span><span>bookworm</span>"
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
      statement: "This year I’d like to feel more",
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
          button: "<span>inquisitive</span>",
          value: "<span>whip-smart</span>"
        },
        {
          id: 4,
          button: "<span>appreciative</span>",
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
      habit: "<span>toilet</span><span>paper</span>",
      button_text: "Let’s wrap it up",
      revisions: [
        {
          id: 1,
          button: "<span>self-love</span>",
          value: "<span>self-love</span>"
        },
        {
          id: 2,
          button: "<span>funny</span><span>memes</span>",
          value: "<span>funny</span><span>memes</span>"
        },
        {
          id: 3,
          button: "<span>empathy</span>",
          value: "<span>empathy</span>"
        },
        {
          id: 4,
          button: "<span>USPS</span><span>stamps</span>",
          value: "<span>USPS</span><span>stamps</span>"
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
import { saveAs } from 'file-saver'

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
      resultIsHidden: true,
      isHidden: true,
      isRevised: false,
      pageUrl: 'https://cahillscreative.com/2020-revised/',
      pageTitle: "Revise your 2020!",
      results: {
        button: [],
        value: [],
        gif: []
      }
    }
  },
  methods: {
    next() {
      setTimeout(() => {
        if (this.resolutionIndex < this.quiz.resolutions.length) {
          this.resolutionIndex++;
        }
        this.isRevised = false;
      }, 300)
    },
    removeClass() {
      document.getElementById("social-share").classList.remove('is-active');
      document.getElementById("social-share").classList.add('is-inactive');
    },
    addClass() {
      document.getElementById("social-share").classList.add('is-active');
    },
    revise(index, resolutionIndex) {
      var id = resolutionIndex + '-' + index;
      document.getElementById("o-revision--" + id).classList.add('is-active');
      setTimeout(() => {
        this.selectedIndex = index;
        this.isRevised = true;
        this.results.button[resolutionIndex] = document.getElementById("o-revision--" + id).dataset.button;
        this.results.value[resolutionIndex] = document.getElementById("o-revision--" + id).dataset.value;
        this.results.gif[resolutionIndex] = document.getElementById("o-revision--" + id).dataset.gif;
      }, 300)
    },
    screenshot() {
      var cardSquare = document.getElementById("card-square");
      html2canvas(cardSquare, {
        width: 600,
        height: 600,
        scrollX: 0,
        scrollY: -window.scrollY
      }).then(function(canvas) {
        canvas.toBlob(function(blob) {
          saveAs(blob, "2020Revised.png");
        });
      });
    },
  }
}
</script>