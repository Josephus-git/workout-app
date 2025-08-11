<script setup lang="ts">
    import { gymHealthFacts } from '../../utils';
    import Grid from '../Grid.vue';
    // generate a random whole integer number between 0 and array length - 1
    const randomNumber = Math.floor(Math.random() * gymHealthFacts.length)
    const todaysFact = gymHealthFacts[randomNumber]
</script>

<template>
    <section id="dashboard">
        <div class ="card tip-container">
            <h2>Welcome Smoldier</h2>
            <div>
            <p class="tip"><strong>Daily Tip</strong><br /> {{todaysFact}}</p>
            </div>
            <button>Start workout &rarr;</button>
        </div>
        <Grid />
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
