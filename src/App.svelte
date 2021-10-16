<script>
  import Canvas from "./Canvas.svelte";
  import Background from "./Background.svelte";
  import FPS from "./FPS.svelte";
  import Score from "./Score.svelte";
  import GameButton from "./GameButton.svelte";
  import SnakeGame from "./SnakeGame.svelte";

  import { color0 } from "./colors";
</script>

<Canvas>
  <Background color={color0} />
  <SnakeGame />
  <FPS />
  <Score />
</Canvas>

<style>
  :global(body) {
    margin: 0;
    padding: 0;
  }
</style>
