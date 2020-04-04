<template>
<div>
  <h1>Trainig</h1>
  <div class="list-div">
    <ol>
      <li v-for="exercise in exercises" v-bind:key="exercise.id">
        <span @click="listPopUp(exercise.id)">
          #{{exercise.id}} {{exercise.name}}
        </span>
        <ol class="ol-inside" v-if="exercise.open === true">
          <li class="li-inside" v-for="occurence in exercise.occurences" v-bind:key="occurence.date" @click="change(exercise.id)">
            {{occurence.date}} -> {{occurence.weight}}kg
          </li>
        </ol>
      </li>
    </ol>
  </div>
</div>
</template>

<script>
export default {
  data() {
    return {
      hover: false,
      clicked: [0, 0, 0],
      exercises: []
    }
  },

  mounted() {
    this.getExercises();
  },

  methods: {
    getExercises() {
      this.test = [
        {"message": "AAA"},
        {"message": "BBB"}
      ],
      this.exercises = [
        {
        "id": 1,
        "name": 'Bench press',
        "open": false,
        "occurences": [
          {
            "date": '01.04.2020',
            "weight": 50
          },
          {
            "date": '15.03.2020',
            "weight": 45
          },
          {
            "date": '02.03.2020',
            "weight": 40
          },
        ]
        },
        {
        "id": 2,
        "name": 'Squat',
        "open": false,
        "occurences": [
          {
            "date": '01.04.2020',
            "weight": 50
          },
          {
            "date": '15.03.2020',
            "weight": 45
          },
          {
            "date": '02.03.2020',
            "weight": 40
          },
        ]
        },
        {
        "id": 3,
        "name": 'Deadlift',
        "open": false,
        "occurences": [
          {
            "date": '01.04.2020',
            "weight": 50
          },
          {
            "date": '15.03.2020',
            "weight": 45
          },
          {
            "date": '02.03.2020',
            "weight": 40
          },
        ]
        }
      ]
    },

    listPopUp(id) {
      this.exercises[id - 1].open = !this.exercises[id - 1].open
    },

    change(id) {
      this.exercises[id].name = "test"
    }

  }
}
</script>

<style scoped>
.list-div {
  width: 70vw;
}

@media screen and (max-width: 768px){
  .list-div {
    width:95vw;
  }
}

h1 {
  margin: 0;
}

ol {
  padding: 0;
  list-style-position: inside;
  list-style-type: none;
  text-align: left;
}

ol li {
  margin: 0rem 0rem 0.5rem 0rem;
  font-size: 1.5rem;
  background: lightcoral;
}

ol li span {
  display: block;
  font-size: 2rem;
  padding: 1rem;
  background: lightcoral;
}

span:hover {
  background: pink;
}

.li-inside {
  background:cornflowerblue;
  border: grey;
  border-style: solid;
  padding: 0.25rem;
  margin-left: 1rem;
}

.li-inside:hover {
  background: lightblue;
}

.ol-inide {
  background:cornflowerblue;
  padding: 1rem;
}

</style>