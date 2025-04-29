<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Pong Game</title>
  <style>
    body { 
      margin: 0; 
      display: flex; 
      justify-content: center; 
      align-items: center; 
      flex-direction: column;
      height: 100vh; 
      background-color: #000; 
      color: white; 
      font-family: Arial, sans-serif;
    }
    canvas { border: 1px solid #fff; }
    .links {
      margin-top: 20px;
      font-size: 18px;
      color: #fff;
    }
    .links a {
      color: #4CAF50; 
      text-decoration: none;
    }
    .links a:hover {
      text-decoration: underline;
    }
    #top-langs {
      margin-top: 20px;
      display: flex;
      justify-content: center;
    }
    #top-langs img {
      border: 1px solid #fff;
    }
  </style>
</head>
<body>

<canvas id="pong" width="600" height="400"></canvas>

  <!-- Links Section -->
  <div class="links">
    <p>Visit my <a href="https://gilkan.github.io/" target="_blank">GitHub.io</a> for more!</p>
  </div>

  <!-- Top Languages Chart Section -->
  <div id="top-langs">
    <img loading="lazy" height="180em" src="https://github-readme-stats.vercel.app/api/top-langs/?username=Gilkan&layout=compact&langs_count=8&theme=dracula"/>
  </div>

  <script>
    const canvas = document.getElementById("pong");
    const context = canvas.getContext("2d");

    const paddleWidth = 10, paddleHeight = 100, ballRadius = 10;
    let leftPaddleY = canvas.height / 2 - paddleHeight / 2, rightPaddleY = canvas.height / 2 - paddleHeight / 2;
    let ballX = canvas.width / 2, ballY = canvas.height / 2, ballSpeedX = 5, ballSpeedY = 4;

    const revealText = (text, positionX, positionY) => {
      context.font = "20px Arial";
      context.fillText(text, positionX, positionY);
    }

    const update = () => {
      // Ball movement
      ballX += ballSpeedX;
      ballY += ballSpeedY;

      if (ballY + ballRadius > canvas.height || ballY - ballRadius < 0) ballSpeedY = -ballSpeedY;

      // Paddle movement
      document.addEventListener('mousemove', (e) => {
        leftPaddleY = e.clientY - canvas.offsetTop - paddleHeight / 2;
      });

      // Ball collision with paddles
      if (ballX - ballRadius < paddleWidth && ballY > leftPaddleY && ballY < leftPaddleY + paddleHeight) ballSpeedX = -ballSpeedX;
      if (ballX + ballRadius > canvas.width - paddleWidth && ballY > rightPaddleY && ballY < rightPaddleY + paddleHeight) ballSpeedX = -ballSpeedX;

      // Reveal text on background (just as an example, this is static for now)
      context.clearRect(0, 0, canvas.width, canvas.height); // Clear the canvas each frame
      revealText("Pedro Vaz", 250, 200); // Position text based on game state

      // Draw paddles and ball
      context.fillRect(0, leftPaddleY, paddleWidth, paddleHeight); // Left paddle
      context.fillRect(canvas.width - paddleWidth, rightPaddleY, paddleWidth, paddleHeight); // Right paddle
      context.beginPath();
      context.arc(ballX, ballY, ballRadius, 0, Math.PI * 2);
      context.fill();
    }

    setInterval(update, 1000 / 60); // 60 FPS update rate
  </script>
</body>
</html>
