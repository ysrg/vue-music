<template>
  <div id="app">
    <header>
      <h1>music app</h1>
    </header>
    <main>
      <section class="player">
        <h2>{{ current.artist }} - {{ current.title }}</h2>
        <div class="controls">
          <button @click="prev">Prev</button>
          <button v-if="!isPlaying" @click="play">Play</button>
          <button v-else @click="pause">Pause</button>
          <button @click="next">Next</button>
        </div>
      </section>
      <section>
        <h3>The playlist</h3>
        <button
          v-for="song in songs"
          :key="song.src"
          @click="play(song)"
          :class="song.src == current.src ? 'song playing' : 'song'"
        >
          {{ song.title }} - {{ song.artist }}
        </button>
      </section>
    </main>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      index: 0,
      isPlaying: false,
      current: {
        title: 'Song Title',
      },
      songs: [
        {
          title: 'Robot Rock',
          artist: 'Daft Punk',
          src: require('./assets/daft-punk-robot-rock.mp3'),
        },
        {
          title: 'Feuer Frei',
          artist: 'Rammstein',
          src: require('./assets/rammstein-feuer-frei.mp3'),
        },
        {
          title: 'Sonne',
          artist: 'Rammstein',
          src: require('./assets/rammstein-sonne.mp3'),
        },
      ],
      player: new Audio(),
    };
  },
  methods: {
    play(song) {
      if (song.src) {
        this.current = song;
        this.player.src = this.current.src;
      }
      this.player.play();
      this.isPlaying = true;
    },
    pause() {
      this.player.pause();
      this.isPlaying = false;
    },
    next() {
      this.index++;
      if (this.index > this.songs.length - 1) this.index = 0;
      this.current = this.songs[this.index];
      if (this.isPlaying) this.play(this.current);
    },
    prev() {
      this.index--;
      if (this.index < 0) this.index = this.songs.length - 1;
      this.current = this.songs[this.index];
      if (this.isPlaying) this.play(this.current);
    },
  },
  created() {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
  },
};
</script>

<style></style>
