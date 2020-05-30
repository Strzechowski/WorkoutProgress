<template>
  <div id="exerciseList">
    <h3>{{trainingName}}</h3>
    <ol>
      <li v-for="(exercise, index) in exercises" v-bind:key="exercise.id">
        <span @click="getCurrentSets(index)">
          #{{index + 1}} {{exercise.name}}
        </span>
        <Sets v-if="editing === index"
          v-bind:sets="sets"
          v-bind:parentExerciseId="exercise.id"
          @delete:set="deleteSet"
          @add:set="postSet"
          @edit:set="putSet"
        />
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
        trainingName: null
    },
    data() {
      return {
        sets: [],
        editing: -1
      }
    },

    methods: {
      getCurrentSets(index) {
        console.log(index)
        if (this.editing === index) {
          this.editing = -1
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

  .li-inside {
    margin-left: 0;
  }
}

</style>
