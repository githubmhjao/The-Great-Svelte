<!-- 在 Javascript 當中 import Counter -->
<script>
  import Counter from './lib/Counter.svelte';
  import Modal from './lib/Modal.svelte';
  import Palettes from './lib/Palettes.svelte';

  let count = 87;
  let showModal = false;

  let palettes = [
    { color: 'ff4000', lock: false, id: 1 },
    { color: '32e6e3', lock: false, id: 2 },
    { color: '009fe9', lock: false, id: 3 }
  ]

  let someState = 'TheGreatSvelte';
  const sparkle = (text) => {
    const sparkles = ['★', '☆', '✧', '✪'];
    const randomSparkles = () => sparkles[Math.floor(Math.random() * sparkles.length)];
    const sparkledText = text.split('').reduce((a, c) => a + randomSparkles() + c, '');
    return sparkledText;
  }

  const href = 'https://ithelp.ithome.com.tw/users/20120178/ironman/7031';

  const handleClick = (e) => {
    console.log(e);
    const tobeCount = count + e.detail;
    if (!(tobeCount > 87)) count = tobeCount;
    else showModal = true;
  }
</script>

<main>
  <!-- 在 HTML 當中直接嵌入 Counter -->
  <Counter {count} on:changeCount={handleClick}/>

  <Palettes />

  <p class='comment'>Check out <a {href}>Svelte Tutorial</a>, the awesome article powered by {sparkle(someState)}!</p>
</main>

<Modal {showModal} on:closeModal={() => showModal = false}/>

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