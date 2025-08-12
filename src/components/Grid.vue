<script setup lang="ts">
    import { computed } from 'vue';
    import { workoutProgram } from '../utils'
    const workoutTypes = ['Push', 'Pull', 'Legs']

    const props = defineProps<{
        handleSelectedWorkout: (workoutIndex: number) => void,
        firstIncompleteWorkoutIndex: number | undefined,
        handleResetPlan: () => void
    }> ();

    const workoutIcons = ['fa-solid fa-dumbbell', 'fa-solid fa-weight-hanging', 'fa-solid fa-bolt'];

    const isWorkoutDisabled = (workoutIndex: number): boolean => {
        // Disable all if the state is not determined yet (e.g., loading)
        if (props.firstIncompleteWorkoutIndex === undefined) {
            return true;
        }
        // Enable all if all workouts are complete
        if (props.firstIncompleteWorkoutIndex === -1) {
            return false;
        }
        // Disable workouts that are after the first incomplete one
        return workoutIndex > props.firstIncompleteWorkoutIndex;
    }

    const isResetDisabled = computed(() => props.firstIncompleteWorkoutIndex !== -1);

</script>

<template>
  <section id="grid">
    <button :disabled="isWorkoutDisabled(workoutIdx)" @click="() => handleSelectedWorkout(workoutIdx)" v-for="(_, workoutIdx) in Object.keys(workoutProgram)" :key="workoutIdx" class="card-button plan-card">
        <div>
            <p>Day {{workoutIdx < 9 ? '0'+ (workoutIdx + 1) : workoutIdx + 1 }}</p>
            <i :class="workoutIcons[workoutIdx%3]"></i>
        </div>
        <h3>{{ workoutTypes[workoutIdx%3] }}</h3>
    </button>
    <button :disabled="isResetDisabled" @click="handleResetPlan" class="card-button plan-card-reset">
        <p>Reset</p>
        <i class="fa-solid fa-rotate-left" > </i>
    </button>
  </section>
</template>

<style scoped>
    #grid{
        display: grid;
        grid-template-columns: repeat(3, minmax(0, 1fr));
        gap: 1rem;
    }

    #grid button {
        width: 100%;
    }

    #grid button:disabled {
        box-shadow: none;
        cursor: not-allowed;
    }

    .plan-card {
        display: flex;
        flex-direction: column;
    }

    .plan-card-reset {
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 1rem;
    }

    .plan-card div {
        display: flex;
        align-items: center;
        justify-content: space-between;
        gap: 1rem;
    }

    .plan-card div p {
        text-align: left;
    }

    @media (min-width: 640px) {
        #grid {
            grid-template-columns: repeat(4, minmax(0, 1fr));
        }
    }

</style>
