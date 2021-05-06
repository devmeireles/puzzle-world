<template>
  <div>
    <input
      type="text"
      maxlength="1"
      v-for="(item, i) in wordArr"
      :key="i"
      :class="`input-${i}-${position}`"
      :id="i"
      data-id=""
    />
  </div>
</template>

<script>
export default {
  name: "Input",
  props: {
    word: {
      type: String,
      required: true,
      default: "",
    },
    position: {
      type: Number,
      required: true,
    },
  },
  data() {
    return {
      wordArr: [],
    };
  },
  created() {
    this.wordArr = this.wordToArray(this.word);
  },
  mounted() {
    window.addEventListener("keyup", (e) => {
      if (e.code === "Backspace") {
        this.delete();
      }

      var charTyped = String.fromCharCode(e.which);
      if (/[a-z]/i.test(charTyped)) {
        this.jumpInput();
      }
    });
  },
  methods: {
    wordToArray(word) {
      return Object.assign([], word);
    },
    jumpInput() {
      const currentID = document.activeElement.id;
      const nextID = Number(currentID) + 1;

      if (nextID < this.word.length) {
        document.getElementById(nextID).focus();
      }

      if (nextID === this.word.length) {
        this.checkWord();
      }
    },
    delete() {
      const currentID = document.activeElement.id;
      const previousID = Number(currentID) - 1;

      if (previousID >= 0) {
        document.getElementById(previousID).focus();
      }
    },
    checkWord() {
      let answerWord = "";
      for (var i = 0; i < this.word.length; i++) {
        answerWord += document.getElementsByClassName(
          `input-${i}-${this.position}`
        )[0].value;
      }

      if (answerWord === this.word) {
        console.log("yes");
        this.$root.$emit("moveTab", this.position + 1);
      }
    },
  },
};
</script>

<style scoped>
input {
  width: 50px;
  height: 50px;
  background: #51b883;
  border: none;
  margin-left: 5px;
  font-size: 30px;
  color: #fff;
  font-weight: bold;
  text-transform: uppercase;
  text-align: center;
}

input:focus {
  outline: none;
}
</style>