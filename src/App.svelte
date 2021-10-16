<script>
  import Canvas from "./Canvas.svelte";
  import Background from "./Background.svelte";
  import FPS from "./FPS.svelte";
  import Score from "./Score.svelte";
  import SnakeGame from "./SnakeGame.svelte";
  import HelpText from "./HelpText.svelte";

  import { color0 } from "./colors";
  
</script>

<Canvas>
  <Background color={color0} />
  <SnakeGame />
  <FPS />
  <Score />
  <HelpText x={40} y={40}/>
</Canvas>

<style>
  :global(body) {
    margin: 0;
    padding: 0;
  }
</style>
