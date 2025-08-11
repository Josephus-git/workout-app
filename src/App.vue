<script setup lang="ts">
  import Layout from './components/layouts/Layout.vue';
  import Welcome from './components/pages/Welcome.vue';
  import Dashboard from './components/pages/Dashboard.vue';
  import Workout from './components/pages/Workout.vue';

  import { ref } from 'vue'
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
  const selectedDisplay = ref(3)
  const data = ref(defaultData)
  const selectedWorkout = ref(2)
</script>

<template>
  <Layout> 
    <!-- Page 1 -->
    <Welcome v-if="selectedDisplay ==  1" />
    <!-- Page 2 -->
    <Dashboard v-if="selectedDisplay == 2 "/>
    <!-- Page 3 -->
    <Workout :data="data" :selected-workout="selectedWorkout" v-if="workoutProgram?.[selectedWorkout]"/>
  </Layout>
</template>

<style scoped>

</style>
