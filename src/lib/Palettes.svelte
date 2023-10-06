<script>
  import { createEventDispatcher } from "svelte";
  import unlock from "../assets/unlock.svg";
  import lock from "../assets/lock.svg";
  export let palettes;

  const dispatch = createEventDispatcher();
  const handleClick = (id) => dispatch("changeLock", id);

  const getHexName = async (hex) => {
    const url = `https://www.thecolorapi.com/id?hex=${hex}`;
    const res = await fetch(url);
    const json = await res.json();
    return json.name.value;
  };
</script>

<div class="palettes">
  {#each palettes as { hex, id, locked } (id)}
    <div class="card">
      <div class="palette" style="background: #{hex}" />
      <div class="hex-code">
        <p>
          {hex}
        </p>
        <p>
          {#await getHexName(hex)}
            wait...
          {:then name}
            {name}
          {/await}
        </p>
      </div>
      <!-- svelte-ignore a11y-no-static-element-interactions a11y-click-events-have-key-events -->
      <div class="lock-icon" on:click={() => handleClick(id)}>
        {#if locked}
          <img src={lock} alt="color-locked" />
        {:else}
          <img src={unlock} alt="color-unlocked" />
        {/if}
      </div>
    </div>
  {:else}
    <div class="card no-color">
      <div>
        <p>No color to show. Please add a color.</p>
      </div>
    </div>
  {/each}
</div>

<style>
  .palettes {
    margin: 1em;
    flex: 1;
    display: grid;
    gap: 0.5em;
  }

  .card {
    padding: 0.5em;
    background: var(--color-bg-3);
    border-radius: var(--border-radius);
    display: grid;
    grid-template-columns: 1fr 2fr 0.5fr;
    align-items: center;
  }

  .card.no-color {
    font-size: 1.5em;
    grid-template-columns: 1fr;
  }

  .card .palette {
    height: 100%;
    border-radius: 0.2em;
  }

  .card .lock-icon {
    display: flex;
    width: 3em;
    height: 3em;
    padding: 0.7em;
    opacity: 0.3;
    cursor: pointer;
  }

  .card .lock-icon:hover {
    opacity: 0.5;
  }
</style>
