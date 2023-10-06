<script>
  import { onMount } from "svelte";
  import { createEventDispatcher } from "svelte";
  export let showModal;

  let dialog;
  const dispatch = createEventDispatcher();
  const handleClose = () => dispatch("closeModal");

  onMount(() => {
    dialog = document.querySelector("dialog");
  });

  $: if (dialog && showModal) dialog.showModal();
  $: if (dialog && !showModal) dialog.close();
</script>

<!-- svelte-ignore a11y-click-events-have-key-events a11y-no-noninteractive-element-interactions -->
<dialog on:close={handleClose} on:click={handleClose}>
  <!-- svelte-ignore a11y-no-static-element-interactions -->
  <div on:click|stopPropagation>
    <div class="title">
      <h2>Forbidden</h2>
      <small><em>adjective</em> for·​bid·​den \fər-ˈbi-dᵊn \</small>
    </div>

    <hr />
    <div class="content">
      <p>
        You have no privilidge to enter domain beyond 87. Please register as
        member so we can sent you some spam email.
      </p>
    </div>
    <hr />
    <!-- svelte-ignore a11y-autofocus -->
    <div class="footer">
      <button autofocus on:click={handleClose}>close modal</button>
    </div>
  </div>
</dialog>

<style>
  dialog {
    color: var(--color-text);
    max-width: 20em;
    border-radius: 0.2em;
    border: none;
    margin: auto;
    box-shadow: 0.5em 0.5em 1.5em rgba(0, 0, 0, 0.1),
      -0.5em -0.5em 1.5em rgba(0, 0, 0, 0.1);
  }
  dialog::backdrop {
    background: rgba(0, 0, 0, 0.4);
  }

  dialog > div {
    padding: 2em;
  }

  dialog > div > div {
    margin: 1em 0;
  }

  button {
    font-size: 1.1em;
    padding: 0.5em;
    border-radius: 0.2em;
    border: none;
    outline: none;
    cursor: pointer;
  }

  button:hover {
    filter: brightness(1.02);
  }
</style>
