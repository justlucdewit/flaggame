<template>
  <div id="app" v-if="currentFlag">
    <div id="game" v-if="gameover === false">
      <div id="lives">
        Lives:
        <span class="red">{{ "❤ ".repeat(lives) }}</span>
        <br />
        Score: {{ score }}
      </div>

      <img id="flag" :src="`/flags/${currentFlag.src}`" alt="The flag" />
      
      <br />

      <div id="options">
        <button
          v-for="option in options"
          :key="option"
          @click="checkGuess(option)"
        >
          {{ option }}
        </button>
      </div>
    </div>
    <div v-else>
      Game over <br />
      Your score was {{ score }} <br />
      <div id="options">
        <button @click="reset()">Retry</button>
      </div>
    </div>
  </div>  
</template>

<script>
import flags from './flags.json';

export default {
  name: 'App',
  components: {},
  data: () => ({
    currentFlag: null,
    options: [],
    lives: 3,
    score: 0,
    gameover: false,
  }),

  mounted() {
    this.setFlag(this.getRandomFlag());
  },

  methods: {
    setFlag(flag) {
      this.currentFlag = flag;

      this.options = [this.currentFlag.name];

      // Fill options with 4 unique random flag names
      while (this.options.length < 4) {
        const randomFlag = this.getRandomFlag();
        
        if (!this.options.includes(randomFlag.name)) {
          this.options.push(randomFlag.name);
        }
      }

      // Randomize the array
      this.options = this.options.sort(() => Math.random() - 0.5);
    },

    getRandomFlag() {
      const flag = flags[Math.floor(Math.random() * flags.length)];
      return flag
    },

    checkGuess(guess) {
      if (guess === this.currentFlag.name) {
        this.score++;
      } else {
        this.lives--;
      }

      if (this.lives === 0) {
        this.gameover = true;
      } else {
        this.setFlag(this.getRandomFlag());
      }
    },

    reset() {
      this.lives = 3;
      this.score = 0;
      this.gameover = false;
      this.setFlag(this.getRandomFlag());
    }
  }
}
</script>

<style lang="scss">
body {
  background: url('https://wallpaperboat.com/wp-content/uploads/2020/12/02/62556/light-color-12.jpg');
}

.red {
  color: red;
}

#app {
  text-align: center;
  margin-top: 50px;
}

#lives {
  margin-bottom: 30px;
}

#flag {
  max-height: calc(100vh - 100px);
  width: 80vw;
  max-width: 400px;
  object-fit: contain;
}

#options {
  display: inline-block;
  margin-top: 50px;
}

button {
  display: block;
  background: white;
  border: 1px solid rgb(36, 144, 206);
  color: rgb(36, 144, 206);
  border-radius: 5px;
  font-weight: bold;
  padding: 10px;
  margin: 10px;
  font-size: large;
  width: 300px;

  // Make the background and color transition smoothly
  transition: all 0.2s, color 0.2s;

  &:hover {
    background: rgb(36, 144, 206);
    color: white;
    cursor: pointer;
  }
}
</style>
