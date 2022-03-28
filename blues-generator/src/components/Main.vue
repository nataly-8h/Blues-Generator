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
    return{
      notes: ["C4", "Eb4", "F4", "Gb4", "G4", "Bb4", "C5",],
    }
  },
  props: {
    msg: String,
  },
  created() {
    // window.addEventListener("keydown", (e) => {
    //   if (e.key == " " && !this.pressed) {
    //     console.log("spacebar");
    //     //play a middle 'C' for the duration of an 8th note
    //     synth.triggerAttackRelease("C4", "8n", now);
    //   }
    // });
    window.addEventListener("keyup", (e) => {
      if (e.key == " " && !this.pressed) {
        console.log(this.notes[1]);
        //play a middle 'C' for the duration of an 8th note
        //synth.triggerAttackRelease("C4", "8n");
        // this.playScale();
        this.attack();
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
    attack(){
      synth.triggerAttackRelease("C4", "8n");
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
