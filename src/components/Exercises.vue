<template>
  <div id="exerciseList">
    <h3>{{trainingName}}</h3>
    <ol>
      <li v-for="(exercise, index) in exercises" v-bind:key="exercise.id">

        <div v-if="editing === index">
          <span class="block" @click="getCurrentSets(index)">
            #{{index + 1}} {{exercise.name}}
          </span>
          <button class="block right" v-if="editing === index" @click="editExercise(exercise)" >
            <i class="fa fa-bars"></i>
          </button>
          <Sets
            v-bind:sets="sets"
            v-bind:parentExerciseId="exercise.id"
            @delete:set="deleteSet"
            @add:set="postSet"
            @edit:set="putSet"
          />
        </div>

        <span v-else @click="getCurrentSets(index)">
          #{{index + 1}} {{exercise.name}}
        </span>

      </li>
      <li>
        <div v-if="editing === 'newExercise'" class="center exerciseColors">
          Creating exercise:
          <div>
            <label>Name: </label>
            <input type="text" v-model="exerciseName">
          </div>
          <button @click="addNewExercise()">ADD</button>
          <button @click="saveExercise()">CANCEL</button>
        </div>
        <span v-else @click="startAddingExercise()">
          + New Exercise
        </span>
      </li>
    </ol>
  </div>
</template>

<script>
import Sets from './Sets.vue'
import axios from 'axios'

export default {
    components: {
      Sets
    },
    props: {
        exercises: Array,
        trainingName: null,
        parentTrainingId: null
    },
    data() {
      return {
        sets: [],
        editing: -1,
        exerciseName: null
      }
    },
    methods: {
      startAddingExercise() {
        this.editing = "newExercise"
      },
      addNewExercise() {
        var newExer = {
          "name": this.exerciseName,
          "training": this.parentTrainingId
        }
        this.editing = null

        this.$emit("add:exercise", newExer);
      },
      editExercise(exerciseToModify) {
        this.$emit("edit:exercise", exerciseToModify)
      },
      saveExercise() {
        this.editing = null
        this.exerciseName = ""
      },
      getCurrentSets(index) {
        if (this.editing === index) {
          this.editing = null
        } else {
          this.sets = this.exercises[index].sets
          this.editing = index
        }
      },
      async postSet(newset) {
        await axios.post("http://127.0.0.1:8000/sets/", newset)
        .then(({data}) => { this.sets.push(data) && console.log(data) })
        .catch(err => console.log(err));
      },

      async deleteSet(setId) {
        await axios.delete(`http://127.0.0.1:8000/sets/${setId}/`)
        .then(this.sets = this.sets.filter(set => set.id != setId))
        .catch(err => console.log(err));
      },

      async putSet(set, repetitions, weight) {
        await axios.put(`http://127.0.0.1:8000/sets/${set.id}/`, {
          "weight": weight,
          "repetitions": repetitions,
          "exercise": this.editing
        })
        .then(({data}) => this.sets = this.sets.map(newSet => (newSet.id === set.id ? data : newSet)))
        .catch(err => console.log(err));
      }
    }

}
</script>

<style scoped>
@media screen and (max-width: 568px){
  #exerciseList {
    width:100vw;
  }
}

.exerciseColors {
  color: #E7E5DF;
}

.center {
  list-style-type: none;
  text-align: center;
  padding: 0;
}

.block {
  display: inline-block;
}

.right {
  margin-top: 2vh;
  float: right;
  position: relative;
}

input {
  width: 7em;
}
</style>
