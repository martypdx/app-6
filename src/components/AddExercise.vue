<template>

  <form class="add-exercise" @submit.prevent="onExerciseAdd">

    <FormControl class="inputs" label="" >

        <FormControl label="Muscles" class="exercise-selector">
          <select 
            v-if="muscleMovements"
            v-model="selectedMuscle"
            class="pulldown"
          >
            <!-- you can set the object as the value -->
            <option 
              v-for="muscle in muscles"
              :key="muscle.id"
              :value="muscle"
            >
              {{ muscle.name }}
            </option>
          </select>
        </FormControl>
        
        <FormControl label="Movements" class="exercise-selector">
          <select
            class="pulldown"
            v-model="selectedMovement"
          >
            <!-- Nice work here combining the selection of one item with secondary list based on that item -->
            <option 
              v-for="movement in muscleMovements[this.selectedMuscle.name]"
              :key="movement.id"
              :value="movement.id"
            >
              {{ movement.name }}
            </option>
          </select>
        </FormControl>

        <FormControl label="Sets" class="exercise-selector">
          <select 
            class="pulldown"
            v-model="sets"
          >
            <!-- Vue feature for number ranges, using :value keeps in Number not string -->
            <option
              v-for="number in 10"
              :key="number"
              :value="number"
            >
              {{ number }}
            </option>
          </select>          
        </FormControl>

        <FormControl label="Reps" class="exercise-selector">
          <select 
            class="pulldown"
            v-model="reps"
          >
            <option
              v-for="number in 10"
              :key="number"
              :value="number"
            >
              {{ number }}
            </option>
          </select>          
        </FormControl>

        <FormControl label="Weight" class="exercise-selector">
          <input id="weight" v-model="weight" required/> lb
        </FormControl>

      </FormControl>

      <button class="add-button">Add</button>

  </form>
</template>

<script>
import FormControl from './FormControl.vue';
export default {
  
  props: {
    movements: Array,
    muscles: Array,
    muscleMovements: Object,
    workout: Object,
    handleAddLog: Function
  },
  components: {
    FormControl
  },
  data() {
    return {
      selectedMuscle: muscleMovements.find(m => m.name === 'abdominals'),
      selectedMovement: null,
      sets: null,
      reps: null,
      weight: null
    };
  },


  methods: {

    onExerciseAdd() {
      const log = {
        workout_id: this.workout.id,
        movement_id: this.selectedMovement.id,
        attempted: this.reps,
        completed: null,
        weight: this.weight
      };
      
      for(let i = 0; i < this.sets; i++){
        this.handleAddLog(log);
      }
      
    }


  }

};
</script>

<style>

.add-button {
  width: 75px;
  border-radius: 10%;
  background-color: var(--gymred);
  color: white;
  text-shadow: 2px 2px black;
  font-size: 22px;
  padding: 4px;
  cursor: pointer;
}
.add-button:hover {
  background-color: darkgray;
}

.inputs {
  display: flex;
  justify-content: space-around;
}

.add-exercise {
  border: 2px solid #C97560;
  background-color: #F7EDEA;
  border-radius: .5em;
  padding: 1em;
  width: minmax(90%, 600px);
  margin: 5px auto;
}

.exercise-selector {
  display: inline-block;
  margin:  .2em;
  padding: 0 .2em;
  border-radius: .3em;
  border: 1px solid #C97560;
}
.pulldown {
  border-radius: .2em;
  width: 8em;
  cursor: pointer;
}
.pulldown:hover {
  background-color: darkgray;
}
#weight {
  width: 4em;
}

@media screen and (max-width: 850px) {
  .add-exercise {
    width: 90%
  }
}

</style>
