<script>
  import { createEventDispatcher } from "svelte";
  import { scale } from "svelte/transition";
  import { elasticOut } from "svelte/easing";
  export let celeb;
  export let showprice;
  export let winner;
  export let active;

  const dispatch = createEventDispatcher();
</script>

<div class="card-outer" class:active>
  <button
    class="card-inner"
    style="background-color:#393939; background-image: url({celeb.image})"
    on:click|once={() => dispatch('select')}>
    <div class="details">
      <h2>
        <a target="_blank" href="https://cameo.com/{celeb.id}">{celeb.name}</a>
      </h2>
      <p class="type">{celeb.type}</p>
    </div>
    {#if showprice}
      <div class="price" class:large={winner}>
        <span in:scale={{ easing: elasticOut, duration: 600 }}>
          ${celeb.price}
        </span>
      </div>
    {/if}
  </button>
</div>

<style>
  .card-outer {
    width: 100%;
    height: 100%;
    transition: opacity 400ms linear;
    opacity: 0.5;
    pointer-events: none;
  }
  .card-inner {
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    background: 50% 0 no-repeat;
    background-size: cover;
    overflow: hidden;
    padding: 0;
    text-align: center;
    border-radius: var(--border-radius);
    box-shadow: 2px 4px 6px rgba(0, 0, 0, 0.2);
  }
  .active{
    pointer-events: all;
    opacity: 1;
  }

  .details {
    position: absolute;
    width: 100%;
    bottom: 0;
    padding: 1em 0.5em 0.5em 0.5em;
    background: linear-gradient(
      to bottom,
      hsl(0deg 0% 0% / 0%),
      hsl(0deg 0% 0% / 60%) 35%,
      hsl(0deg 0% 0% / 90%)
    );
    color: hsl(0deg 0% 100%);
  }

  h2 {
    margin: 0 0 0.2em 0;
  }

  .type {
    margin: 0;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }

  .price {
    position: absolute;
    width: 100%;
    height: 100%;
    left: 0;
    top: 0;
    background-color: rgba(0, 0, 0, 0.3);
    color: white;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 4em;
    font-weight: 700;
  }

  .price.large {
    font-size: 6em;
  }

  @media (min-width: 640px) {
    .card-outer {
      height: 0;
      padding: 0 0 100% 0;
    }
  }
</style>
