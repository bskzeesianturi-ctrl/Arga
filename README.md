<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Gem Cacing</title>
    <style>
        body {
            display: flex;
            flex-direction: column;
            align-items: center;
            background-color: #1a1a1a;
            color: white;
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 20px;
        }

        #game-board {
            background-color: #000;
            display: grid;
            grid-template-columns: repeat(20, 20px);
            grid-template-rows: repeat(20, 20px);
            gap: 1px;
        }

        .snake {
            background-color: #00ff00;
        }

        .food {
            background-color: #ff0000;
        }

        #score {
            font-size: 24px;
            margin-bottom: 10px;
        }
    </style>
</head>
<body>
    <h1>Gem Cacing</h1>
    <div id="score">Skor: 0</div>
    <div id="game-board"></div>

    <script>
        const board = document.getElementById('game-board');
        const scoreDisplay = document.getElementById('score');
        const gridSize = 20;
        let snake = [
            { x: 10, y: 10 }
        ];
        let food = { x: 5, y: 5 };
        let direction = 'right';
        let score = 0;
        let gameLoop;

        // Buat papan permainan
        function createBoard() {
            board.innerHTML = '';
            for (let y = 0; y < gridSize; y++) {
                for (let x = 0; x < gridSize; x++) {
                    const cell = document.createElement('div');
                    cell.style.gridRowStart = y + 1;
                    cell.style.gridColumnStart = x + 1;
                    
                    // Cek apakah ini bagian cacing
                    for (let s of snake) {
                        if (s.x === x && s.y === y) {
                            cell.classList.add('snake');
                        }
                    }

                    // Cek apakah ini makanan
                    if (food.x === x && food.y === y) {
                        cell.classList.add('food');
                    }

                    board.appendChild(cell);
                }
            }
        }

        // Gerakkan cacing
        function moveSnake() {
            let head = { ...snake[0] };

            switch(direction) {
                case 'up': head.y--; break;
                case 'down': head.y++; break;
                case 'left': head.x--; break;
                case 'right': head.x++; break;
            }

            snake.unshift(head);

            // Cek apakah makan makanan
            if (head.x === food.x && head.y === food.y) {
                score++;
                scoreDisplay.textContent = `Skor: ${score}`;
                // Buat makanan baru
                food = {
                    x: Math.floor(Math.random() * gridSize),
                    y: Math.floor(Math.random() * gridSize)
                };
            } else {
                snake.pop();
            }

            // Cek tabrakan
            if (head.x < 0 || head.x >= gridSize || head.y < 0 || head.y >= gridSize) {
                clearInterval(gameLoop);
                alert(`Game Over! Skor akhir: ${score}`);
            }

            // Cek tabrakan dengan tubuh sendiri
            for (let i = 1; i < snake.length; i++) {
                if (head.x === snake[i].x && head.y === snake[i].y) {
                    clearInterval(gameLoop);
                    alert(`Game Over! Skor akhir: ${score}`);
                }
            }

            createBoard();
        }

        // Atur kontrol dengan keyboard
        document.addEventListener('keydown', (e) => {
            if (e.key === 'ArrowUp' && direction !== 'down') {
                direction = 'up';
            } else if (e.key === 'ArrowDown' && direction !== 'up') {
                direction = 'down';
            } else if (e.key === 'ArrowLeft' && direction !== 'right') {
                direction = 'left';
            } else if (e.key === 'ArrowRight' && direction !== 'left') {
                direction = 'right';
            }
        });

        // Mulai permainan
        createBoard();
        gameLoop = setInterval(moveSnake, 200);
    </script>
</body>
</html>
