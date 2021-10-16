<script>
  import { renderable } from "./game.js";
  import { color0, color1, color2, color3, color4 } from "./colors.js";
  import { randomInt } from "./util.js";
  import { score } from "./game.js"
  import _ from "lodash";

  const size = 20;
  const marginLeft = 80;
  const marginTop = 80;
  const lineWidth = 1;

  let timePassed = 0;
  const tick = 15;
  const minX = 0;
  const maxX = 25;
  const minY = 0;
  const maxY = 25;

  const directions = Object.freeze({
    NORTH: 0,
    EAST: 1,
    SOUTH: 2,
    WEST: 3,
  });

  const gameState = Object.freeze({
    NOT_STARTED_YET: 0,
    PLAYING: 1,
    PAUSED: 2,
    GAME_OVER: 3,
  })


  let currentGameState = gameState.NOT_STARTED_YET;

  // let headPos = [randomInt(0, maxX), randomInt(0, maxY)];
  let headPos = [10,5];
  let tailTiles = [];
  let tailLen = 5;
  // let currentBearing = randomInt(directions.NORTH, directions.WEST);
  let currentBearing = directions.WEST;
  let foodLoc = [[5,5]];

  function putFood() {
    foodLoc.push([randomInt(minX, maxX), randomInt(minY, maxY)]);
  }

  function eatFood(food) {
    tailLen += 1;
    _.remove(foodLoc, (current) => _.isEqual(current,food))
    score.update(it => it + 1);

    putFood();
  }

  function checkForFood(head) {

    if (foodLoc.some(item => _.isEqual(item, head))) {
      eatFood(head);
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

    checkForFood([x, y])
    return [x, y];
  }

  function handleKeydown(event) {
    switch (event.key) {
      case "ArrowDown":
        if (currentBearing == directions.NORTH) break;
        currentBearing = directions.SOUTH;
        break;
      case "ArrowLeft":
        if (currentBearing == directions.EAST) break;
        currentBearing = directions.WEST;
        break;
      case "ArrowUp":
        if (currentBearing == directions.SOUTH) break;
        currentBearing = directions.NORTH;
        break;
      case "ArrowRight":
        if (currentBearing == directions.WEST) break;
        currentBearing = directions.EAST;
        break;
    }
  }

  renderable((props, dt) => {
    const { context } = props;

    context.save();
    context.lineWidth = lineWidth;

    for (let y = 0; y < maxY; y++) {
      for (let x = 0; x < maxX; x++) {
        context.beginPath();
        context.rect(
          x + marginLeft + x * size,
          y + marginTop + y * size,
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
      marginTop + headPos[0] * (size + lineWidth),
      marginLeft + headPos[1] * (size + lineWidth),
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
        marginTop + tailTile[0] * (size + lineWidth),
        marginLeft + tailTile[1] * (size + lineWidth),
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
        marginTop + food[0] * (size + lineWidth),
        marginLeft + food[1] * (size + lineWidth),
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

<svelte:window on:keydown={handleKeydown} />

<!-- The following allows this component to nest children -->
<slot />


<!-- 

  Known issues: 
  * you can quickly change direction and reverse the snake if you can manage to squeeze it below one tick duration
  * food can spawn on your tail lol
  * cant die yet

 -->