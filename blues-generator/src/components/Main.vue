<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <p>Press Spacebar to start generating Blues!</p>
    <h3>Need some chords?</h3>
    <br>
     <button class="btn btn-indigo outline" @click="playChords">Play
        </button>
    <!-- <ul>
      <li><a href="https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-babel" target="_blank" rel="noopener">babel</a></li>
      <li><a href="https://github.com/vuejs/vue-cli/tree/dev/packages/%40vue/cli-plugin-eslint" target="_blank" rel="noopener">eslint</a></li>
    </ul> -->
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      notes: ["C3", "Eb3", "F3", "Gb3", "G3", "Bb3", "C4",],
      probability: [
        [0.02, 0.22, 0.14, 0.14, 0.14, 0.2, 0.14], //C4
        [0.14, 0.05, 0.14, 0.2, 0.14, 0.19, 0.14], //Eb4
        [0.14, 0.14, 0.05, 0.14, 0.19, 0.14, 0.2], //F4
        [0.14, 0.14, 0.2, 0.05, 0.19, 0.14, 0.14], //Gb4
        [0.14, 0.14, 0.2, 0.19, 0.05, 0.14, 0.14], //G4
        [0.14, 0.14, 0.14, 0.2, 0.19, 0.05, 0.14], //Bb4
        [0.19, 0.14, 0.14, 0.14, 0.2, 0.14, 0.05], //C5
      ],
      // frequency: ["261.63", "311.13", "349.22", "369.99", "391.99", "466.16", "523.25"],
      frequency: ["130.81", "155.56", "174.61", "185", "196", "233.08", "246.94"],
      last:[0],
      loop: null,
      chords: false,
    };
  },
  props: {
    msg: String,
  },
  created() {

    window.addEventListener("keyup", (e) => {
      if (e.key == " " && !this.pressed) {
        // console.log(this.notes[1]);
        let nextNote = this.calculateNext();
        console.log("Playing: " + this.notes[nextNote] + " Frequency: " + this.frequency[nextNote])
        synth.triggerAttackRelease(this.notes[nextNote], "8n");
      }
    });
  },
  methods: {
    playScale() {
      const now = Tone.now();

      synth.triggerAttackRelease("C4", "8n", now);
      synth.triggerAttackRelease("Eb4", "8n", now + 0.1);
      synth.triggerAttackRelease("F4", "8n", now + 0.6);
      synth.triggerAttackRelease("Gb4", "8n", now + 0.9);
      synth.triggerAttackRelease("G4", "8n", now + 1.2);
      synth.triggerAttackRelease("Bb4", "8n", now + 1.5);
      synth.triggerAttackRelease("C5", "8n", now + 1.8);
    },
    calculateNext(){
     
        let nextNote = parseFloat(Math.random()).toFixed(2) ;
        let sum = this.probability[this.last[0]][0];
        console.log("start" + sum + " " + nextNote);
        let play = 0;
        
        for(let i = 0; i < 7; i++ ){
          console.log(sum + " " + nextNote);
          sum += this.probability[this.last[0]][play];
          if(sum < nextNote){
            play ++;
          }else{
             this.last[0] = play;
            return play;
          }
          
        }
          return play;
      
       
      // }
      // let next = this.probability[0];

    },
    playChords(){
     
     let chordNum = 0;
      const chords = [
        ["B3", "D3", "F#3"], // Bm (vi)
        ["G3", "B3", "D3"], // G (iv)
        ["D3", "F#3", "A3"], // D (i)
        ["A3", "C#3", "E3"], // A (v)
      ];
      const synth = new Tone.PolySynth().toDestination();
      synth.volume.value = -6;
      while(!this.chords){
        new Tone.Loop
        console.log("boton");
        synth.triggerAttackRelease(chords[chordNum], "2m");
        chordNum = (chordNum + 1) % 4;
      } 
         
      
    },
  },
};
import * as Tone from "tone";

//create a synth and connect it to the main output (your speakers)
const synth = new Tone.Synth().toDestination();
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
