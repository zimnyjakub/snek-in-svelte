<script>
  import { renderable } from "./game.js";
  import { cinnabar, englishViolet, ivory, maximumYellow } from "./colors.js";

  const size = 20;
  const marginLeft = 80;
  const marginTop = 80;
  const lineWidth = 1;

  let timePassed = 0;
  const tick = 60;
  const minX = 0;
  const maxX = 50;
  const minY = 0;
  const maxY = 50;

  const directions = Object.freeze({
    NORTH: 0,
    EAST: 1,
    SOUTH: 2,
    WEST: 3,
  });

  let headPos = [5, 5];
  let tailTiles = [];
  let tailLen = 5;
  let currentBearing = directions.NORTH;

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
    return [x, y];
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
        context.strokeStyle = englishViolet;
        context.fillStyle = ivory;
        context.fill();
        context.stroke();
        context.closePath();
      }
    }

    for (let t = 0; t <= tailLen; t++) {
      context.beginPath();
      context.strokeStyle = cinnabar;
      context.fillStyle = maximumYellow;
      context.rect(
        marginTop + headPos[0] * (size + lineWidth),
        marginLeft + headPos[1] * (size + lineWidth),
        size,
        size
      );
      context.fill();
      context.stroke();
      context.closePath();
    }

    context.beginPath();
    context.strokeStyle = cinnabar;
    context.fillStyle = maximumYellow;
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
      context.strokeStyle = cinnabar;
      context.fillStyle = maximumYellow;
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

    if (timePassed % tick === 0) {
      headPos = advance(headPos, currentBearing);
    }

    timePassed += 1;

    context.restore();
  });
</script>

<!-- The following allows this component to nest children -->
<slot />
