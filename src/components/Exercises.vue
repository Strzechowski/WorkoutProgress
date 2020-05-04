<template>
  <div id="exerciseList">
    <h3>{{trainingName}}</h3>
    <ol>
      <li v-for="exercise in exercises" v-bind:key="exercise.id">
        <span @click="getCurrentSets(exercise.id)">
          #{{exercise.id}} {{exercise.name}}
        </span>
        <Sets v-if="editing === exercise.id"
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
        editing: 0
      }
    },

    methods: {
      getCurrentSets(id) {
        if (this.editing == id) {
          this.editing = 0
        } else {
          this.sets = this.exercises[id - 1].sets
          this.editing = id
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

ol {
  padding: 0;
  list-style-position: inside;
  list-style-type: none;
  text-align: left;
}

ol li {
  margin: 0rem 0rem 0.5em 0rem;
  background: lightcoral;
}

ol li span {
  max-block-size: 8vh;
  font-size: 1em;
  display: block;
  padding: 0.5em;
  background: lightcoral;
}

span:hover {
  background: pink;
}

@media screen and (max-width: 568px){
  #exerciseList {
    width:100vw;
  }

  .li-inside {
    margin-left: 0;
  }
}

</style>