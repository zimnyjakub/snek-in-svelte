<script>
  import { renderable } from "./game.js";
  import { color0, color1, color2, color3, color4 } from "./colors.js";
  import { randomInt } from "./util.js";
  import { score, gameState } from "./game.js";
  import _ from "lodash";

  import HudOverlay from "./HudOverlay.svelte";

  const directions = Object.freeze({
    NORTH: 0,
    EAST: 1,
    SOUTH: 2,
    WEST: 3,
  });

  const size = 20;
  const marginTop = 80;
  const lineWidth = 1;

  let timePassed = 0;
  const tick = 15;
  const minX = 0;
  const maxX = 25;
  const minY = 0;
  const maxY = 25;
  let marginLeft = 0;

  let currentGameState = gameState.NOT_STARTED_YET;

  let headPos = [randomInt(0, maxX), randomInt(0, maxY)];
  let tailTiles = [];
  let tailLen = 5;
  let currentBearing = randomInt(directions.NORTH, directions.WEST);
  let foodLoc = [[randomInt(0, maxX), randomInt(0, maxY)]];

  function resetGame() {
    score.set(0);
    currentBearing = randomInt(directions.NORTH, directions.WEST);
    headPos = [randomInt(0, maxX), randomInt(0, maxY)];
    tailTiles = [];
    tailLen = 5;
    currentBearing = randomInt(directions.NORTH, directions.WEST);
    foodLoc = [[randomInt(0, maxX), randomInt(0, maxY)]];
  }

  function putFood() {
    foodLoc.push([randomInt(minX, maxX), randomInt(minY, maxY)]);
  }

  function eatFood(food) {
    tailLen += 1;
    _.remove(foodLoc, (current) => _.isEqual(current, food));
    score.update((it) => it + 1);

    putFood();
  }

  function checkForFood(head) {
    if (foodLoc.some((item) => _.isEqual(item, head))) {
      eatFood(head);
    }
  }

  function checkForCollision(head) {
    if (tailTiles.some((item) => _.isEqual(item, head))) {
      currentGameState = gameState.GAME_OVER;
    }
  }

  function advance(headPos, bearing) {
    let newPos = [headPos];

    switch (bearing) {
      case directions.NORTH:
        newPos = [headPos[0], headPos[1] - 1];
        break;
      case directions.EAST:
        newPos = [headPos[0] + 1, headPos[1]];
        break;
      case directions.SOUTH:
        newPos = [headPos[0], headPos[1] + 1];
        break;
      case directions.WEST:
        newPos = [headPos[0] - 1, headPos[1]];
        break;
    }

    const [x, y] = newPos;
    if (x > maxX - 1) {
      return [minX, y];
    }
    if (x < minX) {
      return [maxX - 1, y];
    }
    if (y > maxY - 1) {
      return [x, minY];
    }
    if (y < minY) {
      return [x, maxY - 1];
    }

    checkForFood([x, y]);
    checkForCollision([x, y]);
    return [x, y];
  }

  function handleKeydown(event) {
    switch (event.key) {
      case "ArrowDown":
        if (currentBearing === directions.NORTH) break;
        currentBearing = directions.SOUTH;
        break;
      case "ArrowLeft":
        if (currentBearing === directions.EAST) break;
        currentBearing = directions.WEST;
        break;
      case "ArrowUp":
        if (currentBearing === directions.SOUTH) break;
        currentBearing = directions.NORTH;
        break;
      case "ArrowRight":
        if (currentBearing === directions.WEST) break;
        currentBearing = directions.EAST;
        break;
      case " ":
        if (
          currentGameState === gameState.NOT_STARTED_YET ||
          currentGameState === gameState.GAME_OVER
        ) {
          resetGame();
          currentGameState = gameState.PLAYING;
          break;
        }
        if (currentGameState === gameState.PLAYING) {
          currentGameState = gameState.PAUSED;
          break;
        }
        if (currentGameState === gameState.PAUSED) {
          currentGameState = gameState.PLAYING;
          break;
        }
        break;
    }
  }

  renderable((props, dt) => {
    const { context, width } = props;
    marginLeft = (width - (size + lineWidth) * maxX) / 2;

    context.save();

    context.lineWidth = lineWidth;
    for (let y = 0; y < maxY; y++) {
      for (let x = 0; x < maxX; x++) {
        context.beginPath();
        context.rect(
          marginLeft + x * (size + lineWidth),
          marginTop + y * (size + lineWidth),
          size,
          size
        );
        context.strokeStyle = color0;
        context.fillStyle = color1;
        context.fill();
        context.stroke();
        context.closePath();
      }
    }

    context.beginPath();
    context.strokeStyle = color2;
    context.fillStyle = color3;
    context.rect(
      marginLeft + headPos[0] * (size + lineWidth),
      marginTop + headPos[1] * (size + lineWidth),
      size,
      size
    );
    context.fill();
    context.stroke();
    context.closePath();

    tailTiles.forEach((tailTile) => {
      context.beginPath();
      context.strokeStyle = color3;
      context.fillStyle = color2;
      context.rect(
        marginLeft + tailTile[0] * (size + lineWidth),
        marginTop + tailTile[1] * (size + lineWidth),
        size,
        size
      );
      context.fill();
      context.stroke();
      context.closePath();
    });

    foodLoc.forEach((food) => {
      context.beginPath();
      context.strokeStyle = color4;
      context.fillStyle = color4;
      context.rect(
        marginLeft + food[0] * (size + lineWidth),
        marginTop + food[1] * (size + lineWidth),
        size,
        size
      );
      context.fill();
      context.stroke();
      context.closePath();
    });

    if (timePassed % tick === 0) {
      if (currentGameState !== gameState.PLAYING) {
        return;
      }

      headPos = advance(headPos, currentBearing);

      if (tailTiles.length != tailLen) {
        tailTiles.push(headPos);
      }

      if (tailTiles.length == tailLen) {
        tailTiles.shift();
      }
    }

    timePassed += 1;

    context.restore();
  });
</script>

<HudOverlay {currentGameState} />
<svelte:window on:keydown={handleKeydown} />

<!-- The following allows this component to nest children -->
<slot />

<!-- 

  Known issues: 
  * you can quickly change direction and reverse the snake if you can manage to squeeze it below one tick duration, but you lose if you cheat so ¯\_(ツ)_/¯
  * food can spawn on your tail lol
  * collision check doesnt work on the right edge for food, tail probably too
 -->
