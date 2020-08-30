<script>
    import Welcome from './screens/Welcome.svelte';
    let state = 'welcome'; // or 'playing' is the alternative
</script>

<style>
    main {
        text-align: center;
        padding: 1em;
        max-width: 800px;
        height: 100%;
        margin: 0 auto;
        display: flex;
        flex-flow: column;
        justify-content: center;
    }
</style>

<main>
    {#if state == 'welcome'}
        <Welcome />
    {:else if state == 'playing'}
        <p>game screen goes here</p>
    {/if}
</main>
