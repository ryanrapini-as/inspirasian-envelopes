<template>
  <div class="container">
    <h3>Buyers Edge InspirAsian</h3>
    <h2 class="mb-3">Lunar New Year - Match the Zodiac Signs!</h2>
    <div class="envelope-grid">
      <envelope
        @onClicked="revealEnvelope(index)"
        :is-flipped="flippedPapers.includes(index)"
        :stop-alex-clicking-so-damn-fast="stopAlexClickingSoDamnFast"
        v-for="(item, index) in papers"
        :key="index"
        :paper="item"
      />
    </div>
  </div>
  <div v-if="showOverlay" class="overlay">
    <div class="overlay-content">
      <h1>🎉 You Win! 🎉</h1>
      <h3>Total clicks: {{ clicks }}</h3>
      <button class="button-19" @click="restart">Play Again</button>
      <ConfettiExplosion />
    </div>
  </div>
</template>

<script>
import Envelope from "./components/Envelope.vue";

export default {
  name: "App",
  components: { Envelope },
  data() {
    return {
      papers: [],
      matchedPapers: [],
      flippedPapers: [],
      score: 0,
      clicks: 0,
      firstMatch: -1,
      stopAlexClickingSoDamnFast: false,
    };
  },
  mounted() {
    this.generateCards();
  },
  computed: {
    showOverlay() {
      return this.matchedPapers.length === 12;
    },
  },
  methods: {
    restart() {
      location.reload();
    },
    revealEnvelope(index) {
      this.clicks++;
      this.flippedPapers.push(index);

      if (this.firstMatch !== -1) {
        const firstMatch = this.papers[this.firstMatch];
        const secondMatch = this.papers[index];

        if (
          firstMatch.slice(1) === secondMatch ||
          secondMatch.slice(1) === firstMatch
        ) {
          console.log(firstMatch, secondMatch);
          this.score++;
          this.matchedPapers.push(index);
          this.matchedPapers.push(this.firstMatch);
          this.firstMatch = -1;
          return;
        }
        this.stopAlexClickingSoDamnFast = true;
        setTimeout(() => {
          this.removeNumberFromFlippedPapers(index);
          this.removeNumberFromFlippedPapers(this.firstMatch);
          this.firstMatch = -1;
          this.stopAlexClickingSoDamnFast = false;
        }, 2500);
      } else {
        this.firstMatch = index;
        return;
      }
    },
    generateCards() {
      const papers = [];
      const envelopeCount = 24;

      let zodiacs = [
        "tiger",
        "monkey",
        "dragon",
        "ox",
        "pig",
        "rabbit",
        "horse",
        "rat",
        "sheep",
        "rooster",
        "snake",
      ];
      let slice = this.getRandomItems(zodiacs, 6);
      zodiacs = slice.concat(slice.map((zodiac) => "*" + zodiac));
      this.papers = this.shuffle(zodiacs);
    },
    shuffle(array) {
      for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]];
      }
      return array; // Now it returns the shuffled array
    },
    removeNumberFromFlippedPapers(numberToRemove) {
      this.flippedPapers = this.flippedPapers.filter(
        (number) => number !== numberToRemove
      );
    },
    getRandomItems(array, count) {
      let result = [];
      let indices = [];

      // Generate an array of unique random indices
      while (indices.length < count) {
        let randomIndex = Math.floor(Math.random() * array.length);
        if (!indices.includes(randomIndex)) {
          indices.push(randomIndex);
        }
      }

      // Select items based on the random indices
      for (let i = 0; i < indices.length; i++) {
        result.push(array[indices[i]]);
      }

      return result;
    },
  },
};
</script>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.mb-3 {
  margin-bottom: 4rem;
}

.envelope-grid {
  display: grid;
  grid-template-columns: repeat(6, 1fr);
  gap: 10px;
  grid-auto-rows: minmax(60px, auto);
}

img {
  width: 200px;
}

button,
input {
  margin-right: 0.5rem;
  margin-bottom: 0.5rem;
}

.overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.7);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 9999;
}

.overlay-content {
  background-color: #fff;
  padding: 20px;
  border-radius: 5px;
  text-align: center;
}

/* CSS */
.button-19 {
  appearance: button;
  background-color: #1899d6;
  border: solid transparent;
  border-radius: 16px;
  border-width: 0 0 4px;
  box-sizing: border-box;
  color: #ffffff;
  cursor: pointer;
  display: inline-block;
  font-family: din-round, sans-serif;
  font-size: 15px;
  font-weight: 700;
  letter-spacing: 0.8px;
  line-height: 20px;
  margin: 0;
  outline: none;
  overflow: visible;
  padding: 13px 16px;
  text-align: center;
  text-transform: uppercase;
  touch-action: manipulation;
  transform: translateZ(0);
  transition: filter 0.2s;
  user-select: none;
  -webkit-user-select: none;
  vertical-align: middle;
  white-space: nowrap;
  width: 100%;
}

.button-19:after {
  background-clip: padding-box;
  background-color: #1cb0f6;
  border: solid transparent;
  border-radius: 16px;
  border-width: 0 0 4px;
  bottom: -4px;
  content: "";
  left: 0;
  position: absolute;
  right: 0;
  top: 0;
  z-index: -1;
}

.button-19,
.button-19:focus {
  user-select: auto;
}

.button-19:hover:not(:disabled) {
  filter: brightness(1.1);
  -webkit-filter: brightness(1.1);
}

.button-19:disabled {
  cursor: auto;
}

.button-19:active {
  border-width: 4px 0 0;
  background: none;
}
</style>
