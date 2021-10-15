<script>
  import { width, height } from "./game.js";

  import Canvas from "./Canvas.svelte";
  import Background from "./Background.svelte";
  import DotGrid from "./DotGrid.svelte";
  import Character from "./Character.svelte";
  import Text from "./Text.svelte";
  import FPS from "./FPS.svelte";
  import Score from "./Score.svelte";
  import SnakeGame from "./SnakeGame.svelte";

  import { color0 } from "./colors";
</script>

<Canvas>
  <Background color={color0} />
  <!-- <DotGrid divisions={30} color='hsla(0, 0%, 100%, 0.5)' /> -->
  <!-- <Character size={10} /> -->
  <!-- <Text
		text='Click and drag around the page to move the character.'
		fontSize={12}
		align='right'
		baseline='bottom'
		x={$width - 20}
		y={$height - 20} /> -->
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
