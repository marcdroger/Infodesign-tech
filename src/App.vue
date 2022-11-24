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
      <h1 class="fade-in-top">This storytelling project is best experienced<span>:</span></h1>
      <ul class="fade-in-top">
        <li>On a mid/large screen. (laptop/computer).</li>
        <li>In a modern browser such as Chrome/Firefox.</li>
        <li>While wearing headphones or sound on.</li>
        <li>In fullscreen mode (press F11 on keyboard).</li>
      </ul>
      <p class="fade-in-bottom--second">All set?</p>
      <audio ref="audio__fade">
        <source src="/audio/fade.mp3" type="audio/mpeg">
      </audio>
      <button class="fade-in-bottom--third" @click="hideOverlay">Dive Into History</button>
    </section>

    <!-- TODO: put in seperate component -->
    <noscript>
      please enable your javascript
    </noscript>

    <Nav />
    <main>
      <header>
        <div class="container">
          <h1 ref="header__title">The Best Games Ever Made<span>.</span></h1>
          <p ref="header__paragraph">Dive into history.</p>
        </div>
      </header>
      <ul class="games">
        <li v-for="game in games" :key="game.id">
          <!-- TODO: add overlay mobile -->
          <video loop preload="metadata">
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
        this.$refs.header__title.classList.add('fade-in-top');
        this.$refs.header__paragraph.classList.add('fade-in-bottom--third');

        //get audio file
        let audio = this.$refs.audio__fade;

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