<script setup>
  import Nav from './components/layout/Nav.vue';
  import Footer from './components/layout/Footer.vue';
</script>

<template>
  <div>
    <Nav />
    <header>
      <div class="container">
        <h1>Hello world</h1>
        <p>Total Games: {{myJson.length}}</p>
      </div>
    </header>
    <main class="games">
      <div class="container">
        <!-- TODO: extra: background game change on mousehover + music? -->
        <ul v-for="game in myJson" :key="game.id">
          <li>
            <img :src="game.boxart" alt={{game.name}} />
            <h2>{{ game.name }}</h2>
            <p>{{ game.platform }}</p>
            <p>{{ game.release }}</p>
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
        myJson: json
      }
    }
  }
</script>