<script>
  import { gameState, renderable, width } from "./game.js";
  import GameButton from "./GameButton.svelte";
  import Text from "./Text.svelte";
  import { tips } from "./wowTips.js";

  export let currentGameState;

  const btnWidth = 600;

  let tip = tips[Math.floor(Math.random() * tips.length)];

  let text = "press space to play snek";
  let btnX = ($width - btnWidth) / 2;
  let btnHeight= 100;

  $: {
    if (currentGameState === gameState.PAUSED) {
      text = tips[Math.floor(Math.random() * tips.length)];
      btnHeight = 200;
    }
  }

  renderable((props) => {
    const { context, width, height } = props;

    switch (currentGameState) {
      case gameState.PLAYING:
        break;

      case gameState.NOT_STARTED_YET:
      case gameState.PAUSED:
        context.fillStyle = "rgba(0,0,0,0.8)";
        context.fillRect(0, 0, width, height);
        break;
    }
  });
</script>

<GameButton
  width={btnWidth}
  height={btnHeight}
  x={btnX}
  y={200}
  {text}
  visible={currentGameState !== gameState.PLAYING}
/>

<slot />
