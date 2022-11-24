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
      video.volume = 0.5;
    })
  })

</script>

<template>
  <div>
    <section class="overlay" ref="overlay">
      <h1>This storytelling project is best experienced:</h1>
      <ul>
        <li>On a mid/large screen. (laptop/computer)</li>
        <li>In a modern browser such as Chrome/Firefox.</li>
        <li>While wearing headphones or sound on.</li>
        <li>In fullscreen mode (press F11 on keyboard)</li>
      </ul>
      <p>All set?</p>
      <audio ref="fade">
        <source src="/audio/fade.mp3" type="audio/mpeg">
      </audio>
      <button @click="hideOverlay">I'm ready</button>
    </section>
    <noscript>
      please enable your javascript
    </noscript>
    <Nav />
    <main>
      <header>
        <div class="container">
          <h1>Top {{games.length}} Games according to Metacritic.com</h1>
          <p><a href="https://www.metacritic.com/browse/games/score/metascore/all/all/filtered" target="_blank">metacritic.com</a> & <a href="https://www.vgchartz.com/">vgchartz.com</a></p>
        </div>
      </header>
      <ul class="games">
        <li v-for="game in games" :key="game.id">
          <!-- TODO: add overlay -->
          <video loop preload="none">
            <source :src="game.video" type="video/mp4" />
            This browser does not support video :(
          </video>
          <div class="container">
            <div>
              <img :src="game.boxart" :alt=game.name />
              <p><span>Platform</span> {{ game.platform }}</p>
              <p title="As of November 2022"><span>Sales</span> {{ game.sales }}</p>
            </div>
            <div class="games__info">
              <h2>{{ game.name }}</h2>
              <p class="games__release">{{ game.release }}</p>
              <p class="games__description"> {{ game.description }}</p>
            </div>
          </div>
        </li>
      </ul>
    </main>
    <Footer />
  </div>
</template>

<script>
  //import json file with games data
  import json from './json/bestGames.json';

  //create the observer api for checking if video is in viewport user
  const observer = new IntersectionObserver(entries => {
    //loop every video
    entries.forEach(entry => {
      //if video is in viewport play background video, otherwise pause video
      if(entry.isIntersecting) {
        entry.target.play();
        entry.target.classList.add('show');
      } else {
        entry.target.pause();
        entry.target.classList.remove('show');
      }
    })
  },
  {
    rootMargin: "-350px",
  }
  );

  export default {
    methods: {
      //function for hiding the overlay on button click
      hideOverlay() {
        //add hide class to overlay
        this.$refs.overlay.classList.add('overlay--hide');

        //get audio file
        let audio = this.$refs.fade;

        //set volume and play audio
        audio.volume = 0.4;
        audio.play();
      }
    },

    data() {
      return {
        games: json,
      }
    }
  }
</script>