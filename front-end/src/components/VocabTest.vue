<template>
    <div>
      <h2>Score: {{ score }} out of {{ this.words.length }}</h2>
      <div style="margin-bottom: 1em;">
        <label>From:
          <select v-model="sourceLang" :disabled="testOver" @change="onLangChange">
            <option v-for="lang in languages" :key="lang.value" :value="lang.value" :disabled="lang.value === targetLang">{{ lang.label }}</option>
          </select>
        </label>
        <label style="margin-left: 1em;">To:
          <select v-model="targetLang" :disabled="testOver" @change="onLangChange">
            <option v-for="lang in languages" :key="lang.value" :value="lang.value" :disabled="lang.value === sourceLang">{{ lang.label }}</option>
          </select>
        </label>
      </div>
      <form action="#" @submit.prevent="onSubmit">
        <div class="ui labeled input fluid">
          <div class="ui label">
            <i :class="getFlagClass(sourceLang)"></i>
            {{ getLangLabel(sourceLang) }}
          </div>
          <input type="text" readonly :disabled="testOver" :value="currWord[sourceLang]"/>
        </div>
        <div class="ui labeled input fluid">
          <div class="ui label">
            <i :class="getFlagClass(targetLang)"></i>
            {{ getLangLabel(targetLang) }}
          </div>
          <input type="text" placeholder="Enter word..." v-model="answer" :disabled="testOver" autocomplete="off" />
        </div>
        <div style="display: flex; align-items: center; gap: 1em; margin-top: 1em;">
          <button class="positive ui button" :disabled="testOver">Submit</button>
          <button class="ui button" @click="resetTest" type="button">Reset</button>
        </div>
      </form>
  
      <p :class="['results', resultClass]">
        <span v-html="result"></span>
      </p>
    </div>
  </template>
  
  <script>
  export default {
    name: 'vocab-test',
    props: {
      words: {
        type: Array,
        required: true
      }
    },
    data() {
      return {
        randWords: [...this.words.sort(() => 0.5 - Math.random())],
        incorrectGuesses: [],
        result: '',
        resultClass: '',
        answer: '',
        score: 0,
        testOver: false,
        languages: [
          { value: 'german', label: 'German', flag: 'germany flag' },
          { value: 'english', label: 'English', flag: 'united kingdom flag' },
          { value: 'vietnamese', label: 'Vietnamese', flag: 'vn flag' },
          { value: 'dutch', label: 'Dutch', flag: 'netherlands flag' }
        ],
        sourceLang: 'german',
        targetLang: 'english'
      };
    },
    computed: {
      currWord: function() {
        return this.randWords.length ? this.randWords[0] : '';
      }
    },
    methods: {
      onLangChange: function() {
        if (this.sourceLang === this.targetLang) {
          // Prevent same language selection
          const other = this.languages.find(l => l.value !== this.sourceLang);
          this.targetLang = other.value;
        }
        this.resetTest();
      },
      getLangLabel(lang) {
        const found = this.languages.find(l => l.value === lang);
        return found ? found.label : lang;
      },
      getFlagClass(lang) {
        const found = this.languages.find(l => l.value === lang);
        return found ? found.flag : '';
      },
      resetTest: function() {
        this.randWords = [...this.words.sort(() => 0.5 - Math.random())];
        this.incorrectGuesses = [];
        this.result = '';
        this.resultClass = '';
        this.answer = '';
        this.score = 0;
        this.testOver = false;
      },
      onSubmit: function() {
        let correct = false;
        if (
          this.currWord &&
          this.currWord[this.targetLang] &&
          this.answer.trim().toLowerCase() === this.currWord[this.targetLang].trim().toLowerCase()
        ) {
          correct = true;
        }
        if (correct) {
          this.flash('Correct!', 'success', { timeout: 1000 });
          this.score += 1;
        } else {
          this.flash('Wrong!', 'error', { timeout: 1000 });
          this.incorrectGuesses.push(this.currWord[this.sourceLang]);
        }

        this.answer = '';
        this.randWords.shift();

        if (this.randWords.length === 0) {
          this.testOver = true;
          this.displayResults();
        }
      },
      displayResults: function() {
        if (this.incorrectGuesses.length === 0) {
          this.result = 'You got everything correct. Well done!';
          this.resultClass = 'success';
        } else {
          const incorrect = this.incorrectGuesses.join(', ');
          this.result = `<strong>You got the following words wrong:</strong> ${incorrect}`;
          this.resultClass = 'error';
        }
      }
    }
  };
  </script>
  
  <style scoped>
  .results {
    margin: 25px auto;
    padding: 15px;
    border-radius: 5px;
  }
  
  .error {
    border: 1px solid #ebccd1;
    color: #a94442;
    background-color: #f2dede;
  }
  
  .success {
    border: 1px solid #d6e9c6;
    color: #3c763d;
    background-color: #dff0d8;
  }
  </style>