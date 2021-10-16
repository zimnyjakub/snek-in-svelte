<script>
  import { gameState, renderable, width } from "./game.js";
  import GameButton from "./GameButton.svelte";

  export let currentGameState;

  const btnWidth = 600;

  renderable((props) => {
    const { context, width, height } = props;

    switch (currentGameState) {
      case gameState.PLAYING:
        break;

      case gameState.NOT_STARTED_YET:
        context.fillStyle = "rgba(0,0,0,0.8)";
        context.fillRect(0, 0, width, height);
        break;
    }
  });
</script>

<GameButton
  width={btnWidth}
  height={100}
  x={($width - btnWidth) / 2}
  y={200}
  text={"press space to play snek"}
/>

<slot />
