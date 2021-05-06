<template>
  <v-container>
    <div>
      <v-tabs v-model="tab" align-with-title v-if="words">
        <v-tabs-slider color="yellow"></v-tabs-slider>

        <v-tab v-for="item in words" :key="item" @click="changeTab(item)">
          {{ item }}
        </v-tab>
      </v-tabs>

      <v-tabs-items v-model="tab">
        <v-tab-item v-for="(item, i) in words" :key="item">
          <v-card flat class="d-flex justify-center align-center">
            <v-card-text v-if="!loading">
              <h1>{{ currentGame.definition }}</h1>
              <Input
                :word="currentGame.word"
                :position="i"
                v-if="currentGame.word"
              />
            </v-card-text>
            <Loading v-else />
          </v-card>
        </v-tab-item>
      </v-tabs-items>
    </div>
  </v-container>
</template>

<script>
import Input from "../components/Input";
import Loading from "../components/ui/Loading";
import axios from "axios";
export default {
  name: "Game",
  components: {
    Input,
    Loading,
  },
  props: {
    words: {
      type: Array,
      default: () => [],
    },
    gameSize: {
      type: Number,
      required: true,
    },
  },
  data() {
    return {
      currentGame: {
        word: "",
        definition: "",
        wordArr: [],
      },
      tab: null,
      loading: true,
    };
  },
  created() {
    this.getWord(this.words[0]);
    this.$root.$on("moveTab", (position) => {
      if (position < this.gameSize) {
        this.getWord(this.words[position]);
        this.tab = position;
      }
    });
  },
  methods: {
    changeTab(item) {
      this.getWord(item);
    },
    async getWord(item) {
      const word = item;
      this.loading = true;
      const req = await axios.get(
        `https://api.dictionaryapi.dev/api/v2/entries/en_US/${word}`
      );

      if (req.status === 200) {
        const { data } = req;

        const item = data[0];

        this.currentGame.word = item.word;
        this.currentGame.definition =
          item.meanings[0].definitions[0].definition;

        this.loading = false;
      }
    },
  },
};
</script>

<style scoped>
.v-card {
  min-height: 300px;
}
</style>