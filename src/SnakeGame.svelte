<script>
  import { renderable } from "./game.js";
  import { cinnabar, englishViolet, ivory, maximumYellow } from "./colors.js";
  import { time } from "./game.js";

  let grid = [
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
    [0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
  ];

  let currentPos = [0, 0];

  const size = 60;
  const marginLeft = 80;
  const marginTop = 80;
  const lineWidth = 1;

  let headPos = [marginLeft, marginTop];

  let timePassed = 0;

  const tick = 60;

  renderable((props, dt) => {
    const { context } = props;

    context.save();
    context.lineWidth = 1;

    for (let y = 0; y < 10; y++) {
      for (let x = 0; x < 10; x++) {
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

    context.beginPath();
    context.strokeStyle = cinnabar;
    context.fillStyle = maximumYellow;
    context.lineWidth = lineWidth;
    context.rect(headPos[0], headPos[1], size, size);
    context.fill();
    context.stroke();
    context.closePath();

    if (timePassed % tick == 0) {
      console.log(timePassed);
      if (timePassed % tick === 0) {
        headPos = [headPos[0] + size + lineWidth, headPos[1]];
      }
    }

    timePassed += 1;

    context.restore();
  });
</script>

<!-- The following allows this component to nest children -->
<slot />
