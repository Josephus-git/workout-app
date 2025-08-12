<script setup lang="ts">
    import { computed } from 'vue';
    import { gymHealthFacts } from '../../utils';
    import Grid from '../Grid.vue';

    // generate a random whole integer number between 0 and array length - 1
   
    const props = defineProps<{
        handleSelectedWorkout: (index: number) => void,
        firstIncompleteWorkoutIndex: number | undefined
    }> ();


    const randomNumber = Math.floor(Math.random() * gymHealthFacts.length)
    const todaysFact = gymHealthFacts[randomNumber]

    const startWorkoutIndex = computed(() => {
        // Default to 0 if the index is undefined or negative.
        return Math.max(0, props.firstIncompleteWorkoutIndex ?? 0);
    });

</script>

<template>
    <section id="dashboard">
        <div class ="card tip-container">
            <h2>Welcome Smoldier</h2>
            <div>
            <p class="tip"><strong>Daily Tip</strong><br /> {{todaysFact}}</p>
            </div>
            <button @click="handleSelectedWorkout(startWorkoutIndex)">Start workout &rarr;</button>
        </div>
        <Grid v-bind="props" />
    </section>
</template>

<style scoped>
    #dashboard,
    .tip-container {
        display: flex;
        flex-direction: column;
    }

    #dashboard {
        gap: 2rem;
    }

    .tip-container {
        gap: 0.5rem;
    }

    @media (min-width: 640px) {
        .tip-container {
            gap: 1rem;
        }
    }

</style>
