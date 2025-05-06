//Space Invaders

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Space Invaders</title>
  <link rel="stylesheet" href="styles.css" />
</head>
<body>
  <header>
    <h1>Space Invaders</h1>
  </header>

  <canvas id="spaceCanvas" width="400" height="400"></canvas>

  <p><a href="index.html">🔙 Back to Portal</a></p>

  <script src="js/space.js"></script>
</body>
</html>

//Space Invaders Game
// js/space.js
const canvas = document.getElementById('spaceCanvas');
const ctx = canvas.getContext('2d');

const player = { x: 200, y: 350, width: 20, height: 20, dx: 5 };
const bullets = [];
const invaders = [];
const rows = 3, cols = 8;
let animationId;

// Create invaders
for (let row = 0; row < rows; row++) {
  for (let col = 0; col < cols; col++) {
    invaders.push({ x: 30 + col * 40, y: 30 + row * 40, width: 20, height: 20 });
  }
}

function drawPlayer() {
  ctx.fillStyle = 'white';
  ctx.fillRect(player.x, player.y, player.width, player.height);
}

function drawInvaders() {
  ctx.fillStyle = 'lime';
  invaders.forEach(inv => ctx.fillRect(inv.x, inv.y, inv.width, inv.height));
}

function drawBullets() {
  ctx.fillStyle = 'yellow';
  bullets.forEach(b => ctx.fillRect(b.x, b.y, b.width, b.height));
}

function update() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);

  // Move and draw
  drawPlayer();
  drawInvaders();
  drawBullets();

  bullets.forEach((b, i) => {
    b.y -= b.dy;
    // Remove off‑screen bullets
    if (b.y < 0) bullets.splice(i, 1);
    // Check collisions
    invaders.forEach((inv, j) => {
      if (b.x < inv.x + inv.width && b.x + b.width > inv.x &&
          b.y < inv.y + inv.height && b.y + b.height > inv.y) {
        invaders.splice(j, 1);
        bullets.splice(i, 1);
      }
    });
  });

  animationId = requestAnimationFrame(update);
}

// Controls
document.addEventListener('keydown', e => {
  if (e.key === 'ArrowLeft' && player.x > 0) player.x -= player.dx;
  if (e.key === 'ArrowRight' && player.x + player.width < canvas.width) player.x += player.dx;
  if (e.key === ' ' || e.key === 'Spacebar') {
    bullets.push({ x: player.x + 8, y: player.y, width: 4, height: 10, dy: 7 });
  }
});

update();

