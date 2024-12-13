<template>
  <Layout>
    <h2>Buyers Edge InspirAsian</h2>
    <div>
      <envelope v-for="(item, index) in papers" :key="index" :is-winner="item.isWinner" />
    </div>
  </Layout>
</template>

<script>
import Envelope from './components/Envelope.vue';

export default {
  name: 'App',
  components: {Envelope},
  data() {
    return {
      papers: [],
    };
  },
  mounted() {
    this.generateCards();
  },
  methods: {
    generateCards() {
      const papers = [];
      let winnerIndex = Math.floor(Math.random() * 6); // Generate a random index for the winner

      for (let i = 0; i < 6; i++) {
        papers.push({
          isWinner: i === winnerIndex, // Set isWinner to true only for the randomly chosen index
        });
      }
      this.papers = papers;
    },
  },
};
</script>

<style scoped>
img {
  width: 200px;
}

button,
input {
  margin-right: 0.5rem;
  margin-bottom: 0.5rem;
}
</style>
