<template>
  <section class="overlay" ref="overlay">
    <h1 class="fade-in-top">This storytelling project is best experienced<span>:</span></h1>
    <ul class="fade-in-top">
      <li>On a mid/large screen. (laptop/computer).</li>
      <li>In a modern browser such as Chrome/Firefox/Safari.</li>
      <li>While wearing headphones or sound on.</li>
      <li>In fullscreen mode (press F11 on Windows/ ⌃⌘F on MacOs).</li>
    </ul>
    <p class="fade-in-bottom--second">All set?</p>
    <audio ref="audio__fade">
      <source src="/audio/fade.mp3" type="audio/mpeg">
    </audio>
    <button class="fade-in-bottom--third" @click="hideOverlay">Dive Into History</button>
  </section>
  <header id="header">
    <div class="container">
      <h1 ref="header__title">The Best Games Ever Made<span>.</span></h1>
      <p ref="header__paragraph">Let's go back in History and search for the top 50 best games ever made. From recent hits to old classics. Nostalgia will be delivered. <br /><span>Scroll down to start the journey</span></p>
    </div>
  </header>
</template>

<script>
  export default {
    name: 'IntroComponent',

    methods: {
      // hide the overlay on button click
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
    }
  }
</script>