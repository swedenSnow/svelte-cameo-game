<script>
  import { onMount } from "svelte";
  import Welcome from "./screens/Welcome.svelte";
  import Game from "./screens/Game.svelte";
  import { select } from "./select.js";
  import { load_image } from "./utils.js";

  let state = "welcome";
  let selection;

  let celebs_promise;

  const start = async (e) => {
    console.log(e);
    const { celebs, lookup } = await celebs_promise;

    selection = select(celebs, lookup, e.detail.category.slug);

    state = "playing";
  };

  const loads_celebs = async () => {
    const res = await fetch("https://cameo-explorer.netlify.app/celebs.json");
    const data = await res.json();

    const lookup = new Map();

    data.forEach((c) => {
      lookup.set(c.id, c);
    });
    // console.log(lookup);
    // console.log(data);
    //! celebs we will have for the game
    const subset = new Set();
    data.forEach((c) => {
      if (c.reviews >= 50) {
        subset.add(c);
        c.similar.forEach((id) => {
          subset.add(lookup.get(id));
        });
      }
    });
    // console.log(subset);
    return {
      celebs: Array.from(subset),
      lookup,
    };
  };
  onMount(() => {
    celebs_promise = loads_celebs();

    //! load icons aswell
    load_image("/icons/right.svg");
    load_image("/icons/wrong.svg");
  });
</script>

<main>
  {#if state == 'welcome'}
    <Welcome on:select={start} />
  {:else if state == 'playing'}
    <Game {selection} on:restart={() => (state = 'welcome')} />
  {/if}
</main>

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
