<template>
  <h1>Text Generator</h1>
  <section id="sentence">
    <label for="sentenceInput">sentence</label><br />
    <input
      id="sentenceInput"
      v-model="sentence"
      @input="
        getPlaceholders();
        replaceSentence();
      "
      type="text"
    />
  </section>
  <section id="sentence-preview">
    <p v-for="(replacedSentence, index) in replacedSentences" :key="index">
      {{ replacedSentence }}
    </p>
  </section>
  <section id="data">
    <button @click="replacingWords.push({})">Add</button>
    <div v-for="(replacingWord, index) in replacingWords" :key="index">
      <template v-for="(placeholder, index) in placeholders" :key="index">
        <input
          v-model="replacingWord[placeholder]"
          @input="replaceSentence()"
          type="text"
        />
      </template>
    </div>
  </section>
</template>

<script>
import { ref } from "vue";
export default {
  name: "App",

  setup() {
    const sentence = ref("");
    const replacedSentences = ref([]);
    const replacingWords = ref([{}]);
    const placeholders = ref([]);

    const getPlaceholders = () => {
      placeholders.value = sentence.value.match(/{([a-zA-Z0-9]*)}/gm);
    };

    const replaceSentence = () => {
      replacedSentences.value = [];
      replacingWords.value.forEach((replacingWord, index) => {
        replacedSentences.value.push(sentence.value);
      });
      placeholders.value.forEach((placeholder) => {
        replacedSentences.value.forEach((replacedSentence, index) => {
          replacedSentence = replacedSentence.replace(
            placeholder,
            replacingWords.value[index][placeholder]
          );
          replacedSentences.value[index] = replacedSentence;
        });
      });
    };

    return {
      sentence,
      placeholders,
      replacingWords,
      getPlaceholders,
      replacedSentences,
      replaceSentence,
    };
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
