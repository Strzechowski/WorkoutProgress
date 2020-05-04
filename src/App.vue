<template>
  <div id="app">
    <Exercises v-bind:exercises="exercises" v-bind:trainingName="trainingName"/>
  </div>
</template>

<script>
import Exercises from './components/Exercises.vue'
import axios from 'axios'

export default {
  name: 'App',
  components: {
    Exercises
  },
  data() {
    return {
      exercises: null,
      trainingName: 'Training A'
    }
  },
  mounted() {
    this.getApiData()
  },

  methods: {
    async getApiData() {
      await axios.get("http://127.0.0.1:8000/exercises/")
      .then(res => (this.exercises = res.data))
      .catch(err => console.log(err));
    }
  }
}
</script>

<style>
#app {
  font-family: Courier, monospace;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  display: flex;
  text-align: center;
  flex-direction: column;
  align-items: center;
  color: #2c3e50;
  font-size: 8vmin;
}

input {
  margin: 0;
  font-size: 0.75em;
  width: 3em;
  text-align: center;
}

.edit {
  background: lemonchiffon;
  background: lightblue;
}

button {
  border: coral;
  background: coral;
  margin: 0.25em;
  padding: 0.5em 0.5em;
  border-radius: 0.25em;
  font-size: 0.7em;
}

</style>
