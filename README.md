<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Mini Flappy Bird</title>
  <style>
    canvas {
      background: skyblue;
      display: block;
      margin: auto;
    }
  </style>
</head>
<body>
<canvas id="game" width="400" height="500"></canvas>

<script>
const canvas = document.getElementById("game");
const ctx = canvas.getContext("2d");

// Jugador
let bird = { x: 50, y: 150, width: 20, height: 20, gravity: 0.6, lift: -10, velocity: 0 };

// Tubos
let pipes = [];
let frame = 0;
let score = 0;

// Controles
document.addEventListener("keydown", () => bird.velocity = bird.lift);
document.addEventListener("click", () => bird.velocity = bird.lift);

function drawBird() {
  ctx.fillStyle = "yellow";
  ctx.fillRect(bird.x, bird.y, bird.width, bird.height);
}

function drawPipes() {
  ctx.fillStyle = "green";
  pipes.forEach(pipe => {
    ctx.fillRect(pipe.x, 0, pipe.width, pipe.top);
    ctx.fillRect(pipe.x, canvas.height - pipe.bottom, pipe.width, pipe.bottom);
  });
}

function updateBird() {
  bird.velocity += bird.gravity;
  bird.y += bird.velocity;

  if (bird.y + bird.height > canvas.height || bird.y < 0) {
    resetGame();
  }
}

function updatePipes() {
  if (frame % 90 === 0) {
    let top = Math.random() * (canvas.height / 2);
    let gap = 120;
    let bottom = canvas.height - top - gap;
    pipes.push({ x: canvas.width, width: 40, top, bottom });
  }

  pipes.forEach(pipe => {
    pipe.x -= 2;

    if (
      bird.x < pipe.x + pipe.width &&
      bird.x + bird.width > pipe.x &&
      (bird.y < pipe.top || bird.y + bird.height > canvas.height - pipe.bottom)
    ) {
      resetGame();
    }

    if (pipe.x + pipe.width === bird.x) {
      score++;
    }
  });

  pipes = pipes.filter(pipe => pipe.x + pipe.width > 0);
}

function resetGame() {
  bird.y = 150;
  bird.velocity = 0;
  pipes = [];
  score = 0;
  frame = 0;
}

function drawScore() {
  ctx.fillStyle = "black";
  ctx.font = "20px Arial";
  ctx.fillText("Puntos: " + score, 10, 30);
}

function loop() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  drawBird();
  drawPipes();
  drawScore();
  updateBird();
  updatePipes();

  frame++;
  requestAnimationFrame(loop);
}

loop();
</script>
</body>
</html> 
