<script>
  import { gameState, renderable, width, score } from "./game.js";
  import GameButton from "./GameButton.svelte";
  import { tips } from "./wowTips.js";

  export let currentGameState;

  const btnWidth = 800;

  let text = "press space to play snek";
  let btnX = ($width - btnWidth) / 2;
  let btnHeight = 100;

  $: btnX = ($width - btnWidth) / 2;
  $: if (currentGameState === gameState.PAUSED) {
    text = tips[Math.floor(Math.random() * tips.length)];
    btnHeight = 300;
  };
  $: if (currentGameState === gameState.GAME_OVER) {
    text = `gg ez, your score: ${$score}`;
    btnHeight = 100;
  };

  renderable((props) => {
    const { context, width, height } = props;

    if (currentGameState !== gameState.PLAYING) {
      context.fillStyle = "rgba(0,0,0,0.8)";
      context.fillRect(0, 0, width, height);
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
