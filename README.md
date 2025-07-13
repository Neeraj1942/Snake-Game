<!DOCTYPE html>

<html lang="en">

<head>

<meta charset="UTF-8" />

<meta name="viewport" content="width=device-width, initial-scale=1" />

<title>Snake Game - Green on Black</title>

<style>

  body {

    background-color: black;

    display: flex;

    justify-content: center;

    align-items: center;

    height: 100vh;

    margin: 0;

    font-family: 'Courier New', Courier, monospace;

    color: #0f0;

  }

  canvas {

    border: 2px solid #0f0;

    background-color: black;

  }

  #score {

    position: absolute;

    top: 20px;

    left: 50%;

    transform: translateX(-50%);

    font-size: 24px;

    color: #0f0;

  }

</style>

</head>

<body>

<div id="score">Score: 0</div>

<canvas id="game" width="400" height="400"></canvas>



<script>

  const canvas = document.getElementById('game');

  const ctx = canvas.getContext('2d');



  const gridSize = 20;

  const tileCount = canvas.width / gridSize;



  let snake = [{ x: 10, y: 10 }];

  let velocity = { x: 0, y: 0 };

  let food = randomPosition();

  let score = 0;

  let gameOver = false;



  function randomPosition() {

    return {

      x: Math.floor(Math.random() * tileCount),

      y: Math.floor(Math.random() * tileCount)

    };

  }



  window.addEventListener('keydown', e => {

    if (e.key === 'ArrowUp' && velocity.y === 0) velocity = { x: 0, y: -1 };

    else if (e.key === 'ArrowDown' && velocity.y === 0) velocity = { x: 0, y: 1 };

    else if (e.key === 'ArrowLeft' && velocity.x === 0) velocity = { x: -1, y: 0 };

    else if (e.key === 'ArrowRight' && velocity.x === 0) velocity = { x: 1, y: 0 };

  });



  function gameLoop() {

    if (gameOver) {

      ctx.fillStyle = '#0f0';

      ctx.font = '40px monospace';

      ctx.textAlign = 'center';

      ctx.fillText('Game Over!', canvas.width / 2, canvas.height / 2 - 20);

      ctx.font = '20px monospace';

      ctx.fillText(`Score: ${score}`, canvas.width / 2, canvas.height / 2 + 20);

      ctx.fillText('Refresh to Play Again', canvas.width / 2, canvas.height / 2 + 50);

      return;

    }



    // Move snake by velocity

    let head = { x: snake[0].x + velocity.x, y: snake[0].y + velocity.y };



    // Wall wrap

    if (head.x < 0) head.x = tileCount - 1;

    else if (head.x >= tileCount) head.x = 0;

    if (head.y < 0) head.y = tileCount - 1;

    else if (head.y >= tileCount) head.y = 0;



    // Check collision with self

    if (snake.some(segment => segment.x === head.x && segment.y === head.y)) {

      gameOver = true;

    }



    snake.unshift(head);



    // Eat food

    if (head.x === food.x && head.y === food.y) {

      score++;

      document.getElementById('score').textContent = 'Score: ' + score;

      food = randomPosition();

    } else {

      snake.pop();

    }



    // Draw background

    ctx.fillStyle = 'black';

    ctx.fillRect(0, 0, canvas.width, canvas.height);



    // Draw food

    ctx.fillStyle = '#0f0';

    ctx.fillRect(food.x * gridSize, food.y * gridSize, gridSize, gridSize);



    // Draw snake

    snake.forEach((segment, index) => {

      ctx.fillStyle = index === 0 ? '#00ff00' : '#008000'; // head bright green, body darker green

      ctx.fillRect(segment.x * gridSize, segment.y * gridSize, gridSize, gridSize);

    });

  }



  setInterval(gameLoop, 100);

</script>

</body>

</html>
