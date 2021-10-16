<script>
  import { gameState, renderable } from "./game.js";

  export let currentGameState;

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

<slot />
