<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <p>Press Spacebar to start generating Blues!</p>
    <h3>Need some chords?</h3>
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
      notes: ["C4", "Eb4", "F4", "Gb4", "G4", "Bb4", "C5"],
      probability: [
        [0.05, 0.19, 0.14, 0.14, 0.14, 0.2, 0.14], //C4
        [0.14, 0.05, 0.14, 0.2, 0.14, 0.19, 0.14], //Eb4
        [0.14, 0.14, 0.05, 0.14, 0.19, 0.14, 0.2], //F4
        [0.14, 0.14, 0.2, 0.05, 0.19, 0.14, 0.14], //Gb4
        [0.14, 0.14, 0.2, 0.19, 0.05, 0.14, 0.14], //G4
        [0.14, 0.14, 0.14, 0.2, 0.19, 0.05, 0.14], //Bb4
        [0.19, 0.14, 0.14, 0.14, 0.2, 0.14, 0.05], //C5
      ],
      frequency: ["261.63", "311.13", "349.22", "369.99", "391.99", "466.16", "523.25"],
      last:[0],
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
      // if(this.last[0] < 0){
      //   console.log("first");
      //   this.last[0] ++;
      //   return 0;
      // }else{
        let nextNote = parseFloat(Math.random()).toFixed(2) ;
        let sum = this.probability[this.last[0]][0];
        // console.log(sum + " " + nextNote);
        let play = 0;
        
        while(sum <= nextNote){
          sum += this.probability[this.last[0]][play];
          play ++;
        }
        this.last[0] = play;
        return play;
      // }
      // let next = this.probability[0];

    }
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
