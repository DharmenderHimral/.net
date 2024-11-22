<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Snake Game</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-color: #f4f4f4;
            margin: 0;
        }
        .game-container {
            background-color: black;
            width: 400px;
            height: 400px;
            position: relative;
            border-radius: 10px;
        }
        .snake {
            position: absolute;
            background-color: green;
            width: 20px;
            height: 20px;
            border-radius: 4px;
        }
        .food {
            position: absolute;
            background-color: red;
            width: 20px;
            height: 20px;
            border-radius: 4px;
        }
        .score {
            position: absolute;
            top: 10px;
            left: 10px;
            color: white;
            font-size: 20px;
        }
    </style>
</head>
<body>

    <div class="game-container" id="game-container">
        <div class="score" id="score">Score: 0</div>
    </div>

    <script>
        const gameContainer = document.getElementById('game-container');
        const scoreElement = document.getElementById('score');
        const gridSize = 20;
        let snake = [{ x: 100, y: 100 }];
        let snakeDirection = { x: gridSize, y: 0 }; // Initially moving to the right
        let food = generateFood();
        let score = 0;
        let gameInterval;
        let gameOver = false;

        // Set up the game loop
        function startGame() {
            gameInterval = setInterval(() => {
                if (gameOver) {
                    clearInterval(gameInterval);
                    alert('Game Over! Your score is ' + score);
                    return;
                }
                moveSnake();
                checkCollision();
                updateGame();
            }, 100);
        }

        // Update the snake and food positions
        function updateGame() {
            const snakeHead = snake[0];
            const snakeElements = document.querySelectorAll('.snake');
            snakeElements.forEach(el => el.remove());
            
            // Create new snake body segments
            snake.forEach(segment => {
                const snakeElement = document.createElement('div');
                snakeElement.classList.add('snake');
                snakeElement.style.left = segment.x + 'px';
                snakeElement.style.top = segment.y + 'px';
                gameContainer.appendChild(snakeElement);
            });

            // Create the food
            const foodElement = document.createElement('div');
            foodElement.classList.add('food');
            foodElement.style.left = food.x + 'px';
            foodElement.style.top = food.y + 'px';
            gameContainer.appendChild(foodElement);
            
            // Update the score
            scoreElement.innerText = 'Score: ' + score;
        }

        // Move the snake
        function moveSnake() {
            const head = { x: snake[0].x + snakeDirection.x, y: snake[0].y + snakeDirection.y };
            snake.unshift(head);

            if (head.x === food.x && head.y === food.y) {
                score += 10;
                food = generateFood(); // Regenerate food
            } else {
                snake.pop(); // Remove the last segment of the snake
            }
        }

        // Check for collisions with walls or the snake itself
        function checkCollision() {
            const head = snake[0];

            // Check wall collision
            if (head.x < 0 || head.x >= gameContainer.offsetWidth || head.y < 0 || head.y >= gameContainer.offsetHeight) {
                gameOver = true;
            }

            // Check collision with itself
            for (let i = 1; i < snake.length; i++) {
                if (head.x === snake[i].x && head.y === snake[i].y) {
                    gameOver = true;
                }
            }
        }

        // Generate a random food position
        function generateFood() {
            const x = Math.floor(Math.random() * (gameContainer.offsetWidth / gridSize)) * gridSize;
            const y = Math.floor(Math.random() * (gameContainer.offsetHeight / gridSize)) * gridSize;
            return { x, y };
        }

        // Change snake direction based on user input
        document.addEventListener('keydown', (event) => {
            if (event.key === 'ArrowUp' && snakeDirection.y === 0) {
                snakeDirection = { x: 0, y: -gridSize };
            } else if (event.key === 'ArrowDown' && snakeDirection.y === 0) {
                snakeDirection = { x: 0, y: gridSize };
            } else if (event.key === 'ArrowLeft' && snakeDirection.x === 0) {
                snakeDirection = { x: -gridSize, y: 0 };
            } else if (event.key === 'ArrowRight' && snakeDirection.x === 0) {
                snakeDirection = { x: gridSize, y: 0 };
            }
        });

        // Start the game
        startGame();
    </script>

</body>
</html>

