<template>
  <div class="hello">
    <h1>{{ currentGame.definition }}</h1>
    <Input :word="currentGame.word" v-if="currentGame.word"/>
  </div>
</template>

<script>
import Input from "./Input";
import axios from "axios";
import randomWords from 'random-words';
export default {
  name: "HelloWorld",
  components: {
    Input,
  },
  props: {
    msg: String,
  },
  data() {
    return {
      currentGame: {
        word: "",
        definition: "",
        wordArr: [],
      },
    };
  },
  created() {
    this.getWord();
  },
  methods: {
    async getWord() {
      const word = randomWords();
      const req = await axios.get(
        `https://api.dictionaryapi.dev/api/v2/entries/en_US/${word}`
      );

      if (req.status === 200) {
        const { data } = req;

        const item = data[0];

        this.currentGame.word = item.word;
        this.currentGame.definition =
          item.meanings[0].definitions[0].definition;
        console.log(data);
      }
    },
  },
};
</script>
