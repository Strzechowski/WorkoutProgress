<template>
<div>
  <h1>Trainig</h1>
  <div class="list-div">
    <ol>
      <li v-for="exercise in exercises" v-bind:key="exercise.id">
        <span @click="listPopUp(exercise.id)">
          #{{exercise.id}} {{exercise.name}}
        </span>
        <ol class="ol-inside" v-if="editing === exercise.id">
          <li  class="li-inside" v-for="occurence in exercise.occurences" v-bind:key="occurence.id">
            <div v-if="editing === exercise.id && editingOccurence === occurence.id">
              <div class="div-in-row">
                <label>Date:</label>
                <input type="text" v-model="occurence.date">
              </div>
              <div class="div-in-row">
                <label>Weight:</label>
                <input type="text" v-model="occurence.weight">
              </div>
              <button @click="save()">SAVE</button>
            </div>
            <div class="" v-else @click="change(exercise.id, occurence.id)">
              {{occurence.date}} -> {{occurence.weight}}kg
            </div>
          </li>
          <li class="li-inside">
            <div @click="startAddingOccurence()">
              + New occurence
            </div>
            <div v-if="editing === exercise.id && editingOccurence === 'addOccurence'">
              <div class="div-in-row">
                <label>Date:</label>
                <input type="text" v-model="newDate">
              </div>
              <div class="div-in-row">
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
    },

    change(id, occurenceId) {
        this.editing = id
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

div {
  padding: 0.25rem;
}

.div-in-row {
  display: inline-block;
  margin-right: 1rem;
}

button {
  border: coral;
  background: coral;
  padding: 0.5rem 1rem;
  border-radius: 0.25rem;
}

span:hover {
  background: pink;
}

input {
  margin: 0;
  font-size: 1rem;
  width: 5rem;
}

.li-inside {
  background:cornflowerblue;
  border: grey;
  border-style: solid;
  padding: 0rem;
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