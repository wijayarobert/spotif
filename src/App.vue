<template>
  <div id="app">
    <header class="flex justify-center align-center p-2">
      <h1 class="text-white">My Music</h1>
    </header>

    <main>
      <section>
        <h2 id="song-title" class="flex justify-center mt-4 uppercase">{{ current.title }} - {{current.artist}}</h2>
        <div class="control flex justify-center">
          <button class="prev m-2 px-4 py-2 bg-blue text-white" @click="prev">Prev</button>
          <button class="play m-2 px-6 py-2 bg-blue-darker text-white" v-if="!isPlaying" @click="play">Play</button>
          <button class="pause m-2 px-4 py-2 bg-blue-darker text-white" v-else @click="pause">Pause</button>
          <button class="next m-2 px-4 py-2 bg-blue text-white" @click="next">Next</button>
        </div>
      </section>
      <section class="playlist">
        <h3 class="m-6 flex justify-center">The Playlist</h3>
        <h4 class="song flex justify-center p-1 cursor-pointer" v-for="song in songs" :key="song.src" @click="play(song)"
        :class="(song.src == current.src) ? 'song playing' : 'song'">
          {{ song.title }} - {{ song.artist}} 
        </h4><br>
      </section>
    </main>

  </div>
</template>

<script>
export default {
  name: 'App',
  data () {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      songs: [
        {
          title: 'Grateful',
          artist: 'Neffex',
          src: require('./assets/neffex-grateful.mp3')
        },
        {
          title: 'Invincible',
          artist: 'Deaf Kev',
          src: require('./assets/src_assets_deaf-kev-invincible.mp3')
        }
      ],
      player: new Audio()
    }
  },
  methods: {
    play (song) {
      if (typeof song.src != "undefined" ) {
        this.current = song;

        this.player.src = this.current.src;
      }

      this.player.play();
      this.isPlaying = true;
    },
    pause () {
      this.player.pause();
      this.isPlaying = false;
    },
    next () {
      this.index++;
      if (this.index > this.songs.length - 1) {
        this.index = 0;
      }

      this.current = this.songs[this.index];

      this.play(this.current);
    },
    prev () {
      this.index--;
      if (this.index < 0 ) {
        this.index = this.songs.length - 1;
      }

      this.current = this.songs[this.index];

      this.play(this.current);
    }
  },
  created () {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
  }
}
</script>

<style>
header {
  background-color: black;
}

.playlist .song.playing {
  color: blueviolet;
}
</style>
