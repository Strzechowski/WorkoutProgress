<template>
<div>
  <h3>Trainig</h3>
  <div class="list-div">
    <ol>
      <li v-for="exercise in exercises" v-bind:key="exercise.id">
        <span @click="listPopUp(exercise.id)">
          #{{exercise.id}} {{exercise.name}}
        </span>
        <ol class="ol-inside" v-if="editing === exercise.id">
          <li class="li-inside" v-for="occurence in exercise.occurences" v-bind:key="occurence.id">
            <div v-if="editing === exercise.id && editingOccurence === occurence.id">
              <div>
                <label>Date:</label>
                <input type="text" v-model="occurence.date">
              </div>
              <div>
                <label>Weight:</label>
                <input type="text" v-model="occurence.weight">
              </div>
              <button @click="save()">SAVE</button>
            </div>
            <div v-else @click="change(occurence.id)">
              {{occurence.date}} -> {{occurence.weight}}kg
            </div>
          </li>
          <li class="li-inside">
            <div @click="startAddingOccurence()">
              + New occurence
            </div>
            <div v-if="editing === exercise.id && editingOccurence === 'addOccurence'">
              <div>
                <label>Date:</label>
                <input type="text" v-model="newDate">
              </div>
              <div>
                <label>Weight:</label>
                <input type="text" v-model="newWeight">
              </div>
              <button @click="addNewOccurence(exercise.id)">ADD</button>
            </div>
          </li>
        </ol>
      </li>
      <li>
        <span @click="startAddingExercise()">
          + New exercise
        </span>
        <div v-if="editing === 'addExercise'">
          <div class="div-in-row">
            <label>Name:</label>
            <input type="text" v-model="newName">
          </div>
          <button @click="addNewExercise()">ADD</button>
        </div>
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
      exercises: [],
      editing: 0,
      editingOccurence: 0,
      newName: 'Exercise',
      newDate: '01.01.2020',
      newWeight: 0
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
            "id": 1,
            "date": '01.04.2020',
            "weight": 50
          },
          {
            "id": 2,
            "date": '15.03.2020',
            "weight": 45
          },
          {
            "id": 3,
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
            "id": 1,
            "date": '01.04.2020',
            "weight": 50
          },
          {
            "id": 2,
            "date": '15.03.2020',
            "weight": 45
          },
          {
            "id": 3,
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
            "id": 1,
            "date": '01.04.2020',
            "weight": 50
          },
          {
            "id": 2,
            "date": '15.03.2020',
            "weight": 45
          },
          {
            "id": 3,
            "date": '02.03.2020',
            "weight": 40
          },
        ]
        }
      ]
    },

    listPopUp(id) {
      this.editing == id ? this.editing = 0 : this.editing = id
      this.editingOccurence = 0
    },

    change(occurenceId) {
      this.editingOccurence = occurenceId
    },

    save() {
      this.editingOccurence = 0
    },

    startAddingExercise() {
      this.editing == 'addExercise' ? this.editing = 0 : this.editing = 'addExercise'
    },

    addNewExercise() {
      var newId = this.exercises.length
      var newExercise = {
        "id": newId + 1,
        "name": this.newName,
        "occurences": []
      }
      this.exercises[newId] = newExercise
      this.editing = 0
    },

    startAddingOccurence() {
      this.editingOccurence == 'addOccurence' ? this.editingOccurence = 0 : this.editingOccurence = 'addOccurence'
    },

    addNewOccurence(id) {
      var newId = this.exercises[id - 1].occurences.length
      var newOccurence = {
        "id": newId + 1,
        "date": this.newDate,
        "weight": this.newWeight
        }
      this.exercises[id - 1].occurences[newId] = newOccurence
      this.editingOccurence = 0
    },

  }
}
</script>

<style scoped>
.list-div {
  width: 70vw;
}

h3 {
  margin: 0;
}

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

button {
  border: coral;
  background: coral;
  padding: 0.5em 0.5em;
  border-radius: 0.25em;
}

span:hover {
  background: pink;
}

input {
  margin: 0;
  font-size: 0.75em;
  width: 5em;
}

.li-inside {
  background:cornflowerblue;
  border: grey;
  border-style: solid;
  padding: 0rem;
  margin-left: 0.5rem;
  font-size: 0.9em;
}

.li-inside:hover {
  background: lightblue;
}

.ol-inide {
  background:cornflowerblue;
  padding: 1rem;
}

@media screen and (max-width: 768px){
  .list-div {
    width:80vw;
  }

}


@media screen and (max-width: 568px){
  .list-div {
    width:100vw;
  }

  .li-inside {
    margin-left: 0;
  }
}

@media screen and (orientation:landscape) {
  ol li span {
    font-size: 0.75em;
    padding: 0.25em;
  }

  .li-inside {
    font-size: 0.70em;
  }
}

</style>