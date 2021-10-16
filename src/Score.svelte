<script>
  import Text from "./Text.svelte";
  import { score } from "./game.js";

</script>

<Text
	text={`score: ${$score}`}
	fontSize=12
	fontFamily='Courier New'
	align='left'
	baseline='top'
	x={120}
	y={20} />

<!-- The following allows this component to nest children -->
<slot />
