<template>
  <div id="tr">
    Hello
    <ul>
      <li v-for="training in trainings" v-bind:key="training.id">
        <span @click="getCurrentSeries(training.id)">
          {{training.date}} {{training.name}}
        </span>
        <Series v-if="editing === training.id" v-bind:series="series"/>
      </li>
    </ul>
  </div>
</template>

<script>
import Series from './Series.vue'

export default {
    components: {
      Series
    },

    props: {
        trainings: Array,
        editing: 0
    },
    data() {
      return {
        series: [],
      }
    },

    methods: {
      getCurrentSeries(id) {
        if (this.editing == id) {
          this.editing = 0
        } else {
          this.series = trainings[id - 1].series
          this.editing = id
        }
      }
    }

}
</script>