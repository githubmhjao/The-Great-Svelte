<script>
  import unlock from "../assets/unlock.svg";
  let palettes = ['ff4000', '32e6e3', '009fe9'];
</script>

<div class="palettes">

  <!-- 第一張色票 -->
  {#each palettes as palette}
    <div class="card">
      <div class="palette" style='background: #{palette}' />
      <div class="hex-code">
        <p>
          {palette}
        </p>
      </div>
      <div class="lock-icon">
        <img src={unlock} alt="color-unlocked" />
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