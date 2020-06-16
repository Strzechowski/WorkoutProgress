<template>
  <div id="app">
    <div id="buttons">
      <button v-if="smallScreen === true && showExercises === true" @click="openTrainings()">Change training</button>
      <button v-if="showLogin != true" @click="logout()">Logout</button>
    </div>
    <Login v-if="showLogin === true" v-on:logIn="openTrainings"/>
    <Trainings v-if="showTrainings === true"
      v-bind:trainings="trainings"
      v-on:chooseTraining="openExercises"
    />
    <Exercises v-if="showExercises === true"
      @add:exercise="postExercise"
      v-bind:parentTrainingId="trainingId"
      v-bind:exercises="exercises"
      v-bind:trainingName="trainingName"
    />
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
      showLogin: true,
      showTrainings: false,
      showExercises: false,
      showSets: false,
      smallScreen: false,
      exercises: null,
      trainings: null,
      trainingId: null,
      trainingName: null
    }
  },

  methods: {
    async getExerciseData(id) {
      await axios.get(`http://127.0.0.1:8000/trainings/${id}/`)
      .then(res => (this.exercises = res.data.exercises ) && (this.trainingName = res.data.name ) && (this.trainingId = res.data.id ))
      .catch(err => console.log(err));
    },
    async getTrainings(user) {
      await axios.get(`http://127.0.0.1:8000/users/${user}/`)
      .then(res => (this.trainings = res.data.trainings))
      .catch(err => console.log(err));
    },
    async postExercise(newExercise) {
      await axios.post("http://127.0.0.1:8000/exercises/", newExercise)
      .then(({data}) => { this.exercises.push(data) && console.log(data) })
      .catch(err => console.log(err));
    },
    openTrainings(username) {
        if (this.trainings === null) { this.getTrainings(username) }
        this.checkViewType()
        this.showTrainings = true
      },
    openExercises(id) {
        this.getExerciseData(id)
        this.checkViewType()
        this.showExercises = true
      },
    // TO DO - if needed
    openSets() {
      this.checkViewType()
      //this.getExerciseData(id)
      this.showSets = true
    },
    logout() {
      location.reload()
    },
    checkViewType() {
      this.showLogin = false
      if(screen.width < 800) {
        this.resetViews()
        this.smallScreen = true
      } else {
        this.smallScreen = false
      }
    },
    resetViews() {
      this.showExercises = false
      this.showSets = false
      this.showTrainings = false
    }
  }
}
</script>

<style>
@media (min-width: 800px) {
  #trainingsList, #exerciseList {
    display: inline-block;
    max-width: 40%;
    margin: 0px 20px;
    vertical-align: top;
    font-size: 5vh;
  }
  h3 {
    height: 10vh;
  }
  #buttons {
    margin-top: 30px;
    max-width: 10%;
    font-size: 5vh;
  }
}


#app {
  font-family: Courier, monospace;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
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

button:hover {
  background: #EFCA49;
}

#buttons {
  display: inline-block;

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
