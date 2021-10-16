<script>
  import Text from "./Text.svelte";
  import { renderable } from "./game.js";
  import { color1, color3 } from "./colors.js";

  export let x = 0;
  export let y = 0;
  export let width = 200;
  export let height = 80;
  export let radius = 20;
  export let text;
  export let visible;

  let r = x + width;
  let b = y + height;

  $: {
    r = x + width;
    b = y + height;
  }

  renderable((props, dt) => {
    const { context, score } = props;

    if (visible) {
        context.beginPath();
        context.moveTo(x + radius, y);
        context.lineTo(r - radius, y);
        context.quadraticCurveTo(r, y, r, y + radius);
        context.lineTo(r, y + height - radius);
        context.quadraticCurveTo(r, b, r - radius, b);
        context.lineTo(x + radius, b);
        context.quadraticCurveTo(x, b, x, b - radius);
        context.lineTo(x, y + radius);
        context.quadraticCurveTo(x, y, x + radius, y);
        context.strokeStyle = color3;
        context.fillStyle = color1;
        context.lineWidth = 6;
        context.fill();
        context.stroke();
        context.closePath();
    }

  });
</script>

<Text
  {text}
  {visible}
  fontSize="24"
  fontFamily="Arial Black"
  align="center"
  baseline="middle"
  color={color3}
  x={x + width / 2}
  y={y + height / 2}
/>

<!-- The following allows this component to nest children -->
<slot />
