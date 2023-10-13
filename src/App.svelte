<!-- 在 Javascript 當中 import Counter、Modal、Palettes -->
<script>
  import { onMount } from "svelte";
  import Counter from "./lib/Counter.svelte";
  import Modal from "./lib/Modal.svelte";
  import Palettes from "./lib/Palettes.svelte";
  import Toast from "./lib/Toast.svelte";
  import { toastStore } from "./lib/toastStore";

  let showModal = false;

  let palettes = [
    { hex: "ff3e00", locked: false, id: "init_01" },
    { hex: "32e6e3", locked: false, id: "init_02" },
    { hex: "009fe9", locked: false, id: "init_03" },
  ];

  $: count = palettes.length;

  const generateTone = () =>
    ("0" + Math.round(255 * Math.random()).toString(16)).slice(-2);
  const generateHex = () =>
    [generateTone(), generateTone(), generateTone()].reduce(
      (a, c) => a + c,
      ""
    );
  const generatePalette = (() => {
    let uuid = 0;
    return () => ({
      hex: generateHex(),
      locked: false,
      id: `${uuid++}`,
    });
  })();

  let someState = "TheGreatSvelte";
  const sparkle = (text) => {
    const sparkles = ["★", "☆", "✧", "✪"];
    const randomSparkles = () =>
      sparkles[Math.floor(Math.random() * sparkles.length)];
    const sparkledText = text
      .split("")
      .reduce((a, c) => a + randomSparkles() + c, "");
    return sparkledText;
  };

  const href = "https://ithelp.ithome.com.tw/users/20120178/ironman/7031";

  const handleClick = (e) => {
    console.log(e);
    const tobeCount = count + e.detail;
    if (0 <= tobeCount && tobeCount < 7) {
      switch (e.detail) {
        case 1:
          palettes = [...palettes, generatePalette()];
          toastStore.addToast({
            action: "plus",
            hex: palettes.slice(-1)[0].hex,
          });
          break;
        case -1:
          toastStore.addToast({
            action: "minus",
            hex: palettes.slice(-1)[0].hex,
          });
          palettes = palettes.slice(0, -1);
          break;
      }
    } else showModal = true;
  };

  onMount(() => {
    const intervalId = setInterval(() => {
      palettes = palettes.map((palette) => {
        if (palette.locked) {
          return palette;
        } else {
          return { ...palette, hex: generateHex() };
        }
      });
    }, 2000);

    return () => clearInterval(intervalId);
  });
</script>

{#each $toastStore as toast}
  <Toast {toast} />
{/each}

<main>
  <!-- 在 HTML 當中直接嵌入 Counter -->
  <Counter {count} on:changeCount={handleClick} />
  <!-- 在 HTML 當中直接嵌入 Palettes -->
  <Palettes bind:palettes />

  <p class="comment">
    Check out <a {href}>Svelte Tutorial</a>, the awesome article powered by {sparkle(
      someState
    )}!
  </p>
</main>
<!-- 在 HTML 當中直接嵌入 Modal -->
<Modal {showModal} on:closeModal={() => (showModal = false)} />

<style>
  main {
    min-height: 100vh;
    max-width: 500px;
    margin: 0 auto;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
  }

  p.comment {
    color: #888;
    margin: 1em 0.5em;
  }

  p.comment a {
    text-decoration: none;
  }
</style>
