<template>
  <div class="hello">
    <div><button @click="audioVisual">TRYING TO GET DATA FROM AUDIO VISUAL INPUT</button>
    </div>
    <div class="allboxes">
    <div class="changeheight0" :style="{'height':data1[0]+'px'}"><p></p></div>
    <div class="changeheight1" :style="{'height':data1[1]+'px'}"><p></p></div>
    <div class="changeheight2" :style="{'height':data1[2]+'px'}"><p></p></div>
    <div class="changeheight3" :style="{'height':data1[3]+'px'}"><p></p></div>
    <div class="changeheight4" :style="{'height':data1[4]+'px'}"><p></p></div>
    <div class="changeheight5" :style="{'height':data1[5]+'px'}"><p></p></div>
    <div class="changeheight6" :style="{'height':data1[6]+'px'}"><p></p></div>
    <div class="changeheight7" :style="{'height':data1[7]+'px'}"><p></p></div>
    <!-- <div class="changeheight8" :style="{'height':data1[8]+'px'}"><p></p></div> -->
    </div>
   <div><audio :ontimeupdate.prop="updateProgress" id="player"></audio> 
   <div class="float"> 
<button @click="play">Play</button>
<button @click="pause">Pause</button>

<progress @click="progressClick" id="seekbar" value="0.2" max="1" style="width:600px;"></progress></div>
<div class="flex1"><div>{{ currentTime  }}</div><div> /  {{ duration }}</div></div>
</div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  data: function() {
    return {
      currentTime: null,
      duration: null,
      data1: [0, 0, 0, 0, 0, 0, 0, 0]
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

  audioVisual() {
    const CLIENT_ID = process.env.VUE_APP_CLIENT_ID;

    var audioElement = document.getElementById('player');
    audioElement.src = `https://api.soundcloud.com/tracks/575588511/stream?client_id=${CLIENT_ID}`
    // audioElement.src = require('../assets/SampleAudio.mp3');
    audioElement.crossOrigin = "anonymous";

    var ctx = new AudioContext();
    var sourceNode = ctx.createMediaElementSource(audioElement);

    var analyser = ctx.createAnalyser();
    sourceNode.connect(analyser);
    analyser.connect(ctx.destination);

    audioElement.play()

    var frequencyData = new Uint8Array(analyser.frequencyBinCount);

    analyser.getByteFrequencyData(frequencyData);

    this.data1 = frequencyData;

    // console.log(frequencyData);



    



      setInterval(function() {
        analyser.getByteFrequencyData(frequencyData);

        function groupAverage(arr, n) {
      var result = [];
      for (var i = 0; i < arr.length;) {
       var sum = 0;
       for(var j = 0; j< n; j++){
      // Check if value is numeric. If not use default value as 0
        sum += +arr[i++] || 0
       }
       result.push(sum/n);
     }
       return result
      }
        let result = groupAverage(frequencyData, 128)

        this.data1 = result;
      }, 500);



},
    progressClick(e) {
      let el = document.getElementById('seekbar');
      var x = e.pageX - el.offsetLeft;
      var startPos = document.getElementById('seekbar').position;
      var xconvert = x/600;
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

.allboxes {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 250px;
}

.changeheight0 {
  display: flex;
  background-color: #5000FF;
  /* border: 1px solid white; */
  color: white;
  width: 20px;
  justify-items: center;
}

.changeheight1 {
  display: flex;
  background-color: #5000FF;
  /* border: 1px solid white; */
  color: white;
  width: 20px;
  justify-items: center;
}

.changeheight2 {
  display: flex;
  background-color: #5000FF;
  /* border: 1px solid white; */
  color: white;
  width: 20px;
  justify-items: center;
}

.changeheight3 {
  display: flex;
  background-color: #5000FF;
  /* border: 1px solid white; */
  color: white;
  width: 20px;
  justify-items: center;
}

.changeheight4 {
  display: flex;
  background-color: #5000FF;
  /* border: 1px solid white; */
  color: white;
  width: 20px;
  justify-items: center;
}

.changeheight5 {
  display: flex;
  background-color: #5000FF;
  /* border: 1px solid white; */
  color: white;
  width: 20px;
  justify-items: center;
}

.changeheight6 {
  display: flex;
  background-color: #5000FF;
  /* border: 1px solid white; */
  color: white;
  width: 20px;
  justify-items: center;
}

.changeheight7 {
  display: flex;
  background-color: #5000FF;
  /* border: 1px solid white; */
  color: white;
  width: 20px;
  justify-items: center;
}

/* .changeheight8 {
  display: flex;
  background-color: #5000FF;
  color: white;
  width: 20px;
  justify-items: center;
} */

.float {
  position: fixed;
  top: 0;
  float:initial;
}
.flex1 {
  display: flex;
  justify-content: center;
}
progress[value] {
  /* Reset the default appearance */
  -webkit-appearance: none;
   appearance: none;
   

  /* width: 250px; */
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
