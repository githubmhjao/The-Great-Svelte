<!-- 在 Javascript 當中 import Counter、Modal、Palettes -->
<script>
  import { onMount } from "svelte";
  import Counter from "./lib/Counter.svelte";
  import Modal from "./lib/Modal.svelte";
  import Palettes from "./lib/Palettes.svelte";

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
      locked: true,
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
          break;
        case -1:
          palettes = palettes.slice(0, -1);
          break;
      }
    } else showModal = true;
  };

  const handleLock = (e) => {
    console.log(e);
    const targetId = e.detail;
    palettes = palettes.map((palette) => {
      if (palette.id === targetId) {
        return { ...palette, locked: !palette.locked };
      } else {
        return palette;
      }
    });
  };

  onMount(() => {
    const intervalId = setInterval(() => {
      palettes = palettes.map((palette) => ({
        ...palette,
        hex: generateHex(),
      }));
    }, 2000);

    return () => clearInterval(intervalId);
  });
</script>

<main>
  <!-- 在 HTML 當中直接嵌入 Counter -->
  <Counter {count} on:changeCount={handleClick} />
  <!-- 在 HTML 當中直接嵌入 Palettes -->
  <Palettes {palettes} on:changeLock={handleLock} />

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
