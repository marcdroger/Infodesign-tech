<script setup>
  import Nav from './components/layout/Nav.vue';
  import Footer from './components/layout/Footer.vue';
</script>

<template>
  <div>
    <Nav />
    <main>
      <header>
        <div class="container">
          <h1>Top {{games.length}} Games according to Metacritic.com</h1>
          <a href="https://www.metacritic.com/browse/games/score/metascore/all/all/filtered" target="_blank">metacritic.com</a>
        </div>
      </header>
      <div class="container">
        <ul class="games">
          <li v-for="game in games" :key="game.id">
            <!-- TODO: play audio effect on hover? -->
            <!-- TODO: add overlay mobile -->
            <img :src="game.boxart" :alt=game.name @mouseenter="play" @mouseleave="stop" />
            <h2>{{ game.name }}</h2>
            <p>{{ game.platform }}</p>
            <p>{{ game.release }}</p>
            <!-- FIXME: hover score scaling -->
            <p class="games__score">{{ game.score }}</p>
            <div class="video">
              <video loop :key="game.id" muted ref="video" :id="game.id">
                <source :src="game.video" type="video/mp4" />
                This browser does not support video :(
              </video>
            </div>
          </li>
        </ul>
      </div>
    </main>
    <Footer />
  </div>
</template>

<script>
  import json from './json/bestGames.json';

  export default {
    data() {
      return {
        games: json,
      }
    },
    methods: {
        play() {
          console.log('enter');
          //this.$refs.video.play();
          console.log(this.$refs.video);
        },
        stop() {
          console.log('leave');
        }
      }
  }
</script>