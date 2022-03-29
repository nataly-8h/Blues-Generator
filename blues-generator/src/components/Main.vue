<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <p>Press Spacebar to start generating Blues!</p>
    <h3>Need some Chords?</h3>
    <br />
    <button class="btn btn-indigo outline" @click="playChords">Play</button>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      notes: ["C3", "Eb3", "F3", "Gb3", "G3", "Bb3", "C4"],
      probability: [
        [0.02, 0.25, 0.25, 0.17, 0.17, 0.7, 0.7], //C4
        [0.25, 0.02, 0.25, 0.17, 0.17, 0.7, 0.7], //Eb4
        [0.17, 0.25, 0.02, 0.25, 0.17, 0.7, 0.7], //F4
        [0.7, 0.17, 0.25, 0.02, 0.25, 0.17, 0.7], //Gb4
        [0.7, 0.7, 0.17, 0.25, 0.02, 0.25, 0.17], //G4
        [0.7, 0.7, 0.17, 0.17, 0.25, 0.02, 0.25], //Bb4
        [0.7, 0.7, 0.17, 0.17, 0.25, 0.25, 0.02], //C5
      ],
      // frequency: ["261.63", "311.13", "349.22", "369.99", "391.99", "466.16", "523.25"],
      frequency: [
        "130.81",
        "155.56",
        "174.61",
        "185",
        "196",
        "233.08",
        "246.94",
      ],
      last: [0],
      loop: null,
      playingChords: false,
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
        console.log(
          "Playing: " +
            this.notes[nextNote] +
            " Frequency: " +
            this.frequency[nextNote]
        );
        synth.triggerAttackRelease(this.notes[nextNote], "8n");
      }
      if (e.key == "p" && !this.pressed) {
        this.playingChords();
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
    calculateNext() {
      let nextNote = parseFloat(Math.random()).toFixed(2);
      let sum = this.probability[this.last[0]][0];
      // console.log("start" + sum + " " + nextNote);
      let play = 0;

      for (let i = 0; i < 7; i++) {
        // console.log(sum + " " + nextNote);
        sum += this.probability[this.last[0]][play];
        if (sum < nextNote) {
          play++;
        } else {
          this.last[0] = play;
          return play;
        }
      }
      return play;

      // }
      // let next = this.probability[0];
    },
    playChords() {
      if (!this.playingChords) {
        const player = new Tone.Player({
          url: "./BluesBackingTrack.mp3",
          autostart: true,
        }).toDestination();
        Tone.loaded().then(() => {
          player.volume.value = -3;
          player.start();
        });
      }
      this.playingChords = true;
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
