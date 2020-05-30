<template>
  <div id="sets">
    <ol>
      <li v-for="(set, index) in sets" v-bind:key="set.id">
        <div v-if="editing === set.id" class="edit">
          Editing set #{{index + 1}}:
          <div>
            <input type="text" v-model="newReps">
            <label>reps X </label>
            <input type="text" v-model="newWeight">
            <label>kg</label>
          </div>
          <button @click="editSet(set)">SAVE</button>
          <button @click="saveSet()">CANCEL</button>
          <button @click="deleteSet(set.id)">DELETE</button>
        </div>
        <div v-else @click="startEditingSet(set)" class="text">
          {{set.repetitions}}reps X {{set.weight}}kg
        </div>
      </li>
      <li>
        <div v-if="editing === 'newSet'" class="edit">
          Creating set:
          <div>
            <input type="text" v-model="newReps">
            <label>reps X </label>
            <input type="text" v-model="newWeight">
            <label>kg</label>
          </div>
          <button @click="addNewSet()">ADD</button>
          <button @click="saveSet()">CANCEL</button>
        </div>
        <div v-else @click="startAddingSet()">
          + New set
        </div>
      </li>
    </ol>
  </div>
</template>

<script>
export default {
    props: {
      sets: Array,
      parentExerciseId: null
    },
    data() {
      return {
        editing: 0,
        newReps: 0,
        newWeight: 0
      }
    },
    methods: {
      saveSet() {
        this.editing = 0
        this.newReps = 0
        this.newWeight = 0
      },

      startEditingSet(set) {
        this.editing = set.id
        this.newReps = set.repetitions
        this.newWeight = set.weight
      },

      startAddingSet() {
        this.editing = 'newSet'
      },

      addNewSet() {
        var newSet = {
          "weight": this.newWeight,
          "repetitions": this.newReps,
          "exercise": this.parentExerciseId
        }
        this.editing = 0

        this.$emit("add:set", newSet);
      },

      deleteSet(setId) {
        this.$emit("delete:set", setId)
        this.editing = 0
      },

      editSet(set) {
        this.$emit("edit:set", set, this.newReps, this.newWeight)
        this.saveSet()
      },
    }
}
</script>

<style scoped>

li {
  background:#D3D0CB;
  padding: 0;
  font-size: 0.9em;
}

li:hover {
  background: #E7E5DF;
}

ol {
  background:#D3D0CB;
  list-style-type: none;
  text-align: center;
  padding: 0;
}

.text {
  padding: 0.25em 0em 0.25em 0em;
}

</style>
