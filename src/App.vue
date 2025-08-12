<script setup lang="ts">
  import Layout from './components/layouts/Layout.vue';
  import Welcome from './components/pages/Welcome.vue';
  import Dashboard from './components/pages/Dashboard.vue';
  import Workout from './components/pages/Workout.vue';

  import { computed, ref, onMounted } from 'vue'
  import { workoutProgram } from './utils'

  const defaultData: Record<number, any> = {}
  for (let workoutIdx in workoutProgram) {
    const workoutData = workoutProgram[workoutIdx]
    defaultData[workoutIdx] = {}

    //nested loop to loop over every exercise within a workout, and initialize the workout data obj
    for (let e of workoutData.workout) {
      defaultData[workoutIdx][e.name] =  ''
    }
  }
  const selectedDisplay = ref(1)
  const data = ref(defaultData)
  
  const selectedWorkout = ref(-1)

  const isWorkoutComplete = computed(() => {
    const currWorkout = data.value?.[selectedWorkout.value]
    if (!currWorkout) { return false } // guard clause to exit function

    const isCompleteCheck = Object.values(currWorkout).every(ex => !!ex)
    console.log('ISCOMPLETE: ', isCompleteCheck)
    return isCompleteCheck
  })

  const firstIncompleteWorkoutIndex = computed(() => {
    const allWorkouts = data.value
    console.log(allWorkouts)
    if (!allWorkouts) {return -1 }

    // loop over every key value pair, and check if the workout is complete or not
    for (const [index, workout] of Object.entries(allWorkouts )) {
      const isComplete = Object.values(workout).every(ex => !!ex)
      if (!isComplete) {
        return parseInt(index)
      }
    }
    return -1 // all are complete
  })

  function handleChangeDisplay(idx: number) {
    selectedDisplay.value = idx
  }

  function handleSelectedWorkout(idx:number) {
    selectedDisplay.value = 3
    selectedWorkout.value = idx
  }

  function handleSaveWorkout() {
    // save the current data snapshot to localstorage so that we can retrieve it next time
    localStorage.setItem('workouts', JSON.stringify(data.value))

    //show the dashboard
    selectedDisplay.value = 2

    //deselect a workout
    selectedWorkout.value = -1
  }

  function handleResetPlan() {
    selectedDisplay.value = 2
    selectedWorkout.value = -1
    data.value = defaultData
    localStorage.removeItem('workouts')
  }

  onMounted(() => {
    if (!localStorage) { return }
    if (localStorage.getItem('workouts')) {
      // only enter if we find some data stored in local storage
      const savedData = JSON.parse(String(localStorage.getItem('workouts')))
      data.value = savedData
      selectedDisplay.value = 2 // if they have data, then we dont want then to be in welcome page
    }

  })

</script>

<template>
  <Layout> 
    <!-- Page 1 -->
    <Welcome :handleChangeDisplay="handleChangeDisplay" v-if="selectedDisplay ==  1" />
    <!-- Page 2 -->
    <Dashboard :handleResetPlan="handleResetPlan" :firstIncompleteWorkoutIndex="firstIncompleteWorkoutIndex" :handleSelectedWorkout="handleSelectedWorkout" v-if="selectedDisplay == 2 "/>
    <!-- Page 3 -->
    <Workout :handleSaveWorkout="handleSaveWorkout" :isWorkoutComplete="isWorkoutComplete" :data="data" :selected-workout="selectedWorkout" v-if="workoutProgram?.[selectedWorkout]"/>
  </Layout>
</template>

<style scoped>

</style>
