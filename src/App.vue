<template>
  <v-app>
    <v-main>
      <Game v-if="!loading" :words="words" />
      <Loading v-else />
    </v-main>
  </v-app>
</template>

<script>
import Game from "./views/Game";
import Loading from "./components/ui/Loading";
import randomWords from "random-words";

export default {
  name: "App",

  components: {
    Game,
    Loading,
  },

  data() {
    return {
      loading: true,
      words: [],
    };
  },

  created() {
    this.getWordList();
  },
  methods: {
    async getWordList() {
      this.loading = true;
      this.words = await randomWords(2);
      this.loading = false;
    },
  },
};
</script>

<style>
body {
  background: #35495e;
}

h1 {
  margin-bottom: 20px;
  line-height: 26px;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #fff;
  margin-top: 60px;
  background: #35495e;
  /* #51b883 */
}
</style>