<template>
  <div id="app" v-on:click="focus()" >
    <!-- <div class="toggle-container">
      <label class="switch">
        <span class="mode-label">{{ isDarkMode ? " " : " " }}</span>
        <input type="checkbox" v-model="isDarkMode" @input="toggleMode" />
        <span class="slider round"></span>
      </label>
    </div> -->
    <Terminal class="terminal" />
  </div>
</template>

<script>
import Terminal from './components/Terminal.vue'

export default {
  name: 'app',
  components: {
    Terminal
  },
  data () {
    return {
      isDarkMode: true
    }
  },
  methods: {
    toggleMode () {
      this.isDarkMode = !this.isDarkMode
    },
    focus () {
      this.$refs.terminal.$refs.prompt.focus()
    }
  }
}
</script>
<style>
/* Your global styles here */
@import 'https://fonts.googleapis.com/css?family=Source+Code+Pro';
@import 'reset-css/reset.css';

html,
body {
  margin: 0;
  overflow-x: hidden;
}
ol, ul {
    list-style: none;
}

#app {
  font-family: 'Source Code Pro', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  font-size: 14px;
  font-weight: bold;
  height: 100%;
  min-width: 100%;
  line-height: 1.35rem;
}

.terminal {
  min-width: 100vw !important;
  padding-right: 20px;
  padding-left: 20px;
}

/* Media Query for Mobile Devices */
@media (max-width: 599px) {
  #app {
    font-size: 2.8vw;
  }
}

@media (min-width: 950px) {
  #app {
    font-size: 2.7vw;
  }
}

/* Media Query for Wider Screens */
@media (min-width: 1000px) {
  #app {
    font-size: 16px;
    padding: 1rem;
  }
}

/* Minimum font size for larger screens */
@media (min-width: 1400px) {
  #app {
    font-size: 16px;
    padding: 1rem;
  }
}
</style>

