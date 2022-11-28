<script setup>
  import { onMounted } from 'vue';

  onMounted(() => {
    //get all games once vue has rendered
    const gamesList = document.querySelectorAll('.games__item');

    //observe every game item
    gamesList.forEach(game => {
      observer.observe(game);
    })
  })
</script>

<template>
  <ul class="games">
    <li v-for="game in games" :key="game.id" class="games__item">
      <video class="games__video" loop preload="metadata">
        <source :src="game.video" type="video/mp4" />
        This browser does not support the background video :(
      </video>
      <div class="container">
        <div>
          <img class="games__boxart" :src="game.boxart" :alt=game.name />
          <p class="games__platform"><span>Platform</span> {{ game.platform }}</p>
          <p class="games__sales" title="As of November 2022"><span>Sales</span> {{ game.sales }}</p>
        </div>
        <div class="games__info">
          <h2 class="games__title">{{ game.name }}</h2>
          <p class="games__release">{{ game.release }}</p>
          <p class="games__description"> {{ game.description }}</p>
        </div>
      </div>
    </li>
  </ul>
</template>

<script>
  //import json file with games data
  import json from './../../json/bestGames.json';

  //create the observer api for checking if game item is in viewport user
  const observer = new IntersectionObserver(entries => {
    //loop through every game
    entries.forEach(entry => {
      if(entry.isIntersecting) {
        //add transition class to current viewing game
        entry.target.classList.add('games--enter');

        //prevents users having tinnitus from video audio
        entry.target.firstElementChild.volume = 0.5;

        //play background video
        entry.target.firstElementChild.play();
        entry.target.firstElementChild.classList.add('show');
      } else {
        //remove transition class
        entry.target.classList.remove('games--enter');

        //pause current playing video if user leaves section
        entry.target.firstElementChild.pause();
        entry.target.firstElementChild.classList.remove('show');
      }
    })
  },
  {
    rootMargin: "-300px",
  }
  );


  export default {
    name: 'GamesComponent',

    data() {
      return {
        games: json,
      }
    }
  }

</script>