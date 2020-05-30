<template>
  <div id="app">
    <Login v-if="state === 'needsLogin'" v-on:logIn="openTrainings"/>
    <Trainings v-else-if="state === 'training'" v-bind:trainings="trainings" v-on:chooseTraining="openTrainings"/>
    <div id="exercisesDiv" v-else>
      <button @click="openTrainings()">Change training</button>
      <button @click="logout()">Logout</button>
      <Exercises  v-bind:exercises="exercises" v-bind:trainingName="trainingName"/>
    </div>
  </div>
</template>

<script>
import Trainings from './components/Trainings.vue'
import Exercises from './components/Exercises.vue'
import Login from './components/Login.vue'

import axios from 'axios'

export default {
  name: 'App',
  components: {
    Exercises,
    Trainings,
    Login
  },
  data() {
    return {
      state: 'needsLogin',
      exercises: null,
      trainings: null,
      trainingId: null,
      trainingName: null
    }
  },

  methods: {
    async getExerciseData(id) {
      await axios.get(`http://127.0.0.1:8000/trainings/${id}/`)
      .then(res => (this.exercises = res.data.exercises ) && (this.trainingName = res.data.name ))
      .catch(err => console.log(err));
    },
    async getTrainings(user) {
      await axios.get(`http://127.0.0.1:8000/users/${user}/`)
      .then(res => (this.trainings = res.data.trainings))
      .catch(err => console.log(err));
    },
    openTrainings(IdOrUsername) {
      if (this.state != "training") {
        this.state = "training"
        this.getTrainings(IdOrUsername)
      } else {
        this.getExerciseData(IdOrUsername)
        this.state = "exercises"
      }
    },
    logout() {
      this.state = "needsLogin"
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
  background: #E7E5DF;
}

button {
  border:#E6AF2E;
  background: #E6AF2E;
  margin: 0.25em;
  padding: 0.5em 0.5em;
  border-radius: 0.25em;
  font-size: 0.7em;
}

ol {
  padding: 0;
  list-style-position: inside;
  list-style-type: none;
  text-align: left;
}

ol li {
  margin: 0rem 0rem 0.5em 0rem;
  background: #393E41;
}

ol li span {
  font-size: 1em;
  color: #E7E5DF;
  display: block;
  padding: 0.5em;
  background: #393E41;
}

span:hover {
  background: #0A0903;
}


</style>
