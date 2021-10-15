<script>
  import Text from "./Text.svelte";
  import { time, renderable } from "./game.js";

  let text = "score: 0";
  renderable((state, dt) => {
    const { score } = state;
    text = `score: ${score}`;
    
  });
</script>

<Text
	{text}
	fontSize=12
	fontFamily='Courier New'
	align='left'
	baseline='top'
	x={120}
	y={20} />

<!-- The following allows this component to nest children -->
<slot />
