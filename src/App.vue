<template>
  <div id="app">
    <header>
      <h1>My Music</h1>
    </header>
    <main>
      <section class="player">
        <h2 class="song-title"> {{current.title}} - <span>{{current.artist}} </span></h2>
        <div class="controls">
          <button class="prev" @click="previous" >Previous</button>
          <button class="play" v-if="!isplaying" @click="play">Play</button>
          <button class="pause" v-else @click="pause"> Pause</button>
          <button class="next" @click="next" >Next</button>
          
        </div>
      </section>
      <section class="playlist">
        <h2> The Playlist</h2>
        <button v-for="song in songs" :key="song.src" @click="play(song)" :class="(song.src == current.src) ? 'song playing' : 'song'">
          {{song.title}} - {{song.artist}}
        </button>
      </section>
    </main>

  </div>


</template>

<script>
// import func from '../vue-temp/vue-editor-bridge';


export default {
  name: 'App',
    data() {
      return {
        current: {},
        index: 0,
        isplaying: false,
        songs: [
          {
            title:'Scam 1992',
            artist:'Achnit',
            src: require('./assets/Scam 1992.mp3')
          },
          
          {
            title:'Bille Jean',
            artist:'Michael Jackson',
            src: require('./assets/Bille Jean.mp3')
          },
          {
            title:'Hosh Walon Ko Khabar Kiya',
            artist:'Jagjit Singh',
            src: require('./assets/Hosh Walon Ko Khabar Kiya.mp3')
          }
        ],
          
          player: new Audio()
        }
      },

      methods: {
        play(song) {
          if(typeof song.src != "undefined") {
            this.current = song;

            this.player.src = this.current.song;
          }
            this.player.play();
            this.player.addEventListener('ended', function() {
              this.index++;
            
            if(this.index > this.songs.length - 1) {
            this.index = 0;
            }
          this.current =  this.songs[this.index];
          this.play(this.current);
            }.bind(this));
            this.isplaying = true;
        },
        pause() {
            this.player.pause();
            this.isplaying = false;
          }
        },
        next() {
          this.index++;
          if(this.index > this.songs.length - 1) {
            this.index = 0;
            }
          this.current =  this.songs[this.index];
          this.play(this.current);
        },
        prev() {
          this.index--;
          if(this.index < 0 ) {
            this.index = this.songs.length - 1;
            }
          this.current =  this.songs[this.index];
          this.play(this.current);

        },
    

      created() {
          this.current = this.songs[this.index];
          this.player.src = this.current.src;
          //this.player.play();
        
      },
      
      playVideo() {
        const media = this.videoplayer.nativeElement;
        media.muted = true; // without this line it's not working although I have "muted" in HTML
        media.play();
      }
    }

</script>

<style>

* {
    margin: 0;
    padding: 0;
    box-sizing: border-box ;
}

body {
  font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
}

header{
  display: flex;
  justify-content:center;
  align-items: center;
  width: 100%;
  padding: 15px;
  background-color: black;
  color: white;
}

main{
  width: 100%;
  max-width: 768px;
  margin: 0 auto;
  padding: 25px;
}
.song-title{
  color: black;
  font-size: 32px;
  font-weight: 700;
  text-transform: uppercase;
  text-align: center;
}

.song-title span {
  font-weight: 500;
  font-style: italic;
}

.controls{
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 30px 15px;

}

button{
  appearance: none;
  background: none;
  border: none;
  outline: none;
  cursor: none;
}
button:hover{
  opacity: 0.7;
}

.play, .pause{
  font-size: 20px;
  font-weight: 700;
  padding: 15px 25px;
  margin: 0px 15px;
  border-radius: 8px;
  color: white;
  background-color: pink;

}

.next, .prev {
  font-size: 16px;
  font-weight: 700;
  padding: 15px 20px;
  margin: 0px 15px;
  border-radius: 6px;
  color: white;
  background-color: red;
}

.playlist{
  padding: 0px 30px;

}

.playlist h2 {
  font-size: 30px;
  font-weight: 400;
  color: black;
  text-align: center;
  margin-bottom: 30px;
}

.playlist .song {
    display: block;
    width: 100%;
    padding: 15px;
    font-size: 20px;
    font-weight: 700;
    cursor: pointer;
}

.playlist .song:hover{
  color:brown;
}

.playlist .song.playing {
    color:white;
    background-image: linear-gradient(to right, blue, pink);
}










</style>
