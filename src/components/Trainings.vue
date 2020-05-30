<template>
  <div id="trainingsList">
    <h3>Pick a training</h3>
    <ol>
      <li v-for="training in trainings" v-bind:key="training.id">
        <span @click="getCurrentTraining(training.id)">
          {{training.date}} {{training.name}}
        </span>
      </li>
    </ol>
  </div>
</template>

<script>

export default {
    props: {
        trainings: Array,
    },
    data() {
      return {
        series: [],
        editing: 0
      }
    },

    methods: {
      getCurrentTraining(id) {
        this.$emit("chooseTraining", id)
      }
    }

}
</script>

<style scoped>

@media screen and (max-width: 568px){
  #trainingsList {
    width:100vw;
  }

  .li-inside {
    margin-left: 0;
  }
}


</style>
