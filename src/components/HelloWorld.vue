<template>
  <div class="hello">
   <div><audio :ontimeupdate.prop="updateProgress" src="http://ia902606.us.archive.org/35/items/shortpoetry_047_librivox/song_cjrg_teasdale_64kb.mp3" id="player"></audio>  
<button @click="play">Play</button>
<button @click="pause">Pause</button>

<progress @click="progressClick" id="seekbar" value="0" max="1" style="width:400px;"></progress></div>
<div class="flex1"><div>{{ currentTime  }}</div><div> /  {{ duration }}</div></div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data: function() {
    return {
      currentTime: null,
      duration: null
    }
  },
  props: {
    msg: String
  },
  methods: {
    play() {
    document.getElementById('player').play();
},

    pause() {
    document.getElementById('player').pause();
},
    skipTime() {
      var player = document.getElementById('player')

      player.currentTime = 20;
},

    progressClick(e) {
      let el = document.getElementById('seekbar');
      var x = e.pageX - el.offsetLeft;
      var startPos = document.getElementById('seekbar').position;
      var xconvert = x/400;
      var finalx = (xconvert).toFixed(1);
      document.getElementById('seekbar').value = finalx
     
      var player = document.getElementById('player');
      var finalseconds = xconvert*player.duration;
      player.currentTime = finalseconds;
},
     updateProgress() {
    var player = document.getElementById('player');
    var progressbar = document.getElementById('seekbar');
    progressbar.value = (player.currentTime / player.duration);
    // convert time
     const convertTime = (setTime) => {
    var date = new Date(null);
    date.setSeconds(setTime);
    var utc = date.toUTCString();
    return utc.substr(utc.indexOf(':') - 2, 8);
    }
    this.currentTime = convertTime(player.currentTime);
    this.duration = convertTime(player.duration)
}
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
/* .hello {
background-image: url("../assets/alps-background.jpg")
} */

.flex1 {
  display: flex;
  justify-content: center;
}
progress[value] {
  /* Reset the default appearance */
  -webkit-appearance: none;
   appearance: none;
   

  width: 250px;
  height: 100vh;
}
/* add box shadow */
progress[value]::-webkit-progress-bar {
  background-color: rgba(252, 70, 4, 0.0);
  border-radius: 2px;
  
  /* box-shadow: 0 2px 5px rgba(0, 0, 0, 0.0) inset; */
}
/* add funky background */
progress[value]::-webkit-progress-value {
  background-color: rgba(252, 70, 4, 0.4);

border-right:2px solid red;
  -moz-box-sizing:border-box;
  -webkit-box-sizing:border-box;
  box-sizing:border-box;

    border-radius: 1px; 
    background-size: 35px 20px, 100% 100%, 100% 100%;
}
</style>
