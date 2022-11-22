<script setup>
  import { onMounted } from 'vue';

  import Nav from './components/layout/Nav.vue';
  import Footer from './components/layout/Footer.vue';

  //get all background videos once vue has rendered
  onMounted(() => {
    const videos = document.querySelectorAll('video');

    //observe every video
    videos.forEach(video => {
      observer.observe(video);
      //prevents users having tinnitus
      video.volume = 0.3;
    })
  })

</script>

<template>
  <div>
    <Nav />
    <main>
      <header>
        <div class="container">
          <h1>Top {{games.length}} Games according to Metacritic.com</h1>
          <p><a href="https://www.metacritic.com/browse/games/score/metascore/all/all/filtered" target="_blank">metacritic.com</a> & <a href="https://www.vgchartz.com/">vgchartz.com</a></p>
          <button>check</button>
        </div>
      </header>
      <ul class="games">
        <li v-for="game in games" :key="game.id">
          <!-- TODO: add overlay -->
          <p class="games__score">{{ game.score }}</p>
          <video loop preload="metadata">
            <source :src="game.video" type="video/mp4" />
            This browser does not support video :(
          </video>
          <div class="container">
            <img :src="game.boxart" :alt=game.name @mouseenter="play" @mouseleave="stop" />
            <h2>{{ game.name }}</h2>
            <p>{{ game.platform }}</p>
            <p>{{ game.release }}</p>
            <p>{{ game.sales }}</p>
          </div>
        </li>
      </ul>
    </main>
    <Footer />
  </div>
</template>

<script>
  import json from './json/bestGames.json';

  //create the observer api for checking if video is in viewport user
  const observer = new IntersectionObserver(entries => {
    //loop every video
    entries.forEach(entry => {
      //if video is in viewport play video, otherwise pause video
      if(entry.isIntersecting) {
        entry.target.play();
      } else {
        entry.target.pause();
      }
    })
  },
  {
    rootMargin: "-550px",
  }
  );

  export default {
    data() {
      return {
        games: json,
      }
    }
  }
</script>