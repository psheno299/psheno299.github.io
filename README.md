
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>2048</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            background-color: #faf8ef;
            margin: 0;
            padding: 20px;
        }
        
        h1 {
            font-size: 40px;
            margin: 0;
            color: #776e65;
        }
        
        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            width: 450px;
            margin-left: auto;
            margin-right: auto;
        }
        
        .scores {
            display: flex;
            gap: 10px;
        }
        
        .score-container {
            background-color: #bbada0;
            color: white;
            padding: 5px 15px;
            border-radius: 5px;
            font-weight: bold;
            text-align: center;
            min-width: 60px;
        }
        
        .score-title {
            font-size: 14px;
        }
        
        .score-value {
            font-size: 20px;
        }
        
        .game-container {
            width: 450px;
            height: 450px;
            background-color: #bbada0;
            border-radius: 6px;
            margin: 0 auto;
            padding: 15px;
            box-sizing: border-box;
            position: relative;
        }
        
        .grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            grid-template-rows: repeat(4, 1fr);
            gap: 15px;
            width: 100%;
            height: 100%;
        }
        
        .cell {
            background-color: #cdc1b4;
            border-radius: 3px;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 35px;
            font-weight: bold;
            color: #776e65;
        }
        
        .tile {
            position: absolute;
            width: 97.5px;
            height: 97.5px;
            border-radius: 3px;
            display: flex;
            justify-content: center;
            align-items: center;
            font-size: 35px;
            font-weight: bold;
            transition: all 0.1s ease-in-out;
            z-index: 1;
        }
        
        .tile-2 {
            background-color: #eee4da;
        }
        
        .tile-4 {
            background-color: #ede0c8;
        }
        
        .tile-8 {
            background-color: #f2b179;
            color: white;
        }
        
        .tile-16 {
            background-color: #f59563;
            color: white;
        }
        
        .tile-32 {
            background-color: #f67c5f;
            color: white;
        }
        
        .tile-64 {
            background-color: #f65e3b;
            color: white;
        }
        
        .tile-128 {
            background-color: #edcf72;
            color: white;
            font-size: 30px;
        }
        
        .tile-256 {
            background-color: #edcc61;
            color: white;
            font-size: 30px;
        }
        
        .tile-512 {
            background-color: #edc850;
            color: white;
            font-size: 30px;
        }
        
        .tile-1024 {
            background-color: #edc53f;
            color: white;
            font-size: 25px;
        }
        
        .tile-2048 {
            background-color: #edc22e;
            color: white;
            font-size: 25px;
        }
        
        .game-over {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(238, 228, 218, 0.73);
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            z-index: 100;
            border-radius: 6px;
        }
        
        .game-over-text {
            font-size: 60px;
            font-weight: bold;
            color: #776e65;
            margin-bottom: 30px;
        }
        
        button {
            background-color: #8f7a66;
            color: white;
            border: none;
            border-radius: 3px;
            padding: 10px 20px;
            font-size: 18px;
            cursor: pointer;
            font-weight: bold;
            margin: 5px;
        }
        
        button:hover {
            background-color: #9f8b77;
        }
        
        .modal {
            display: none;
            position: fixed;
            z-index: 200;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0,0,0,0.5);
        }
        
        .modal-content {
            background-color: #faf8ef;
            margin: 15% auto;
            padding: 20px;
            border-radius: 5px;
            width: 300px;
            box-shadow: 0 4px 8px rgba(0,0,0,0.2);
        }
        
        .close {
            color: #aaa;
            float: right;
            font-size: 28px;
            font-weight: bold;
            cursor: pointer;
        }
        
        .close:hover {
            color: black;
        }
        
        .saved-games-list {
            max-height: 300px;
            overflow-y: auto;
            margin: 15px 0;
        }
        
        .saved-game {
            padding: 10px;
            margin: 5px 0;
            background-color: #eee4da;
            border-radius: 3px;
            cursor: pointer;
        }
        
        .saved-game:hover {
            background-color: #ede0c8;
        }
        
        .save-name-input {
            width: 100%;
            padding: 8px;
            margin: 10px 0;
            box-sizing: border-box;
            border-radius: 3px;
            border: 1px solid #bbada0;
        }
    </style>
</head>
<body>
    <div class="header">
        <h1>2048</h1>
        <div class="scores">
            <div class="score-container">
                <div class="score-title">Счет</div>
                <div class="score-value" id="score">0</div>
            </div>
            <div class="score-container">
                <div class="score-title">Время</div>
                <div class="score-value" id="timer">0:00</div>
            </div>
        </div>
    </div>
    
    <div class="game-container" id="game-container">
        <div class="grid" id="grid">
            <div class="cell"></div>
            <div class="cell"></div>
            <div class="cell"></div>
            <div class="cell"></div>
            <div class="cell"></div>
            <div class="cell"></div>
            <div class="cell"></div>
            <div class="cell"></div>
            <div class="cell"></div>
            <div class="cell"></div>
            <div class="cell"></div>
            <div class="cell"></div>
            <div class="cell"></div>
            <div class="cell"></div>
            <div class="cell"></div>
            <div class="cell"></div>
        </div>
    </div>

    <div>
        <button id="new-game-btn">Новая игра</button>
        <button id="save-game-btn">Сохранить игру</button>
        <button id="load-game-btn">Загрузить игру</button>
    </div>
    
    <!-- Модальное окно для сохранения игры -->
    <div id="save-modal" class="modal">
        <div class="modal-content">
            <span class="close">&times;</span>
            <h2>Сохранение игры</h2>
            <input type="text" id="save-name" class="save-name-input" placeholder="Введите название сохранения">
            <button id="confirm-save">Сохранить</button>
        </div>
    </div>
    
    <!-- Модальное окно для загрузки игры -->
    <div id="load-modal" class="modal">
        <div class="modal-content">
            <span class="close">&times;</span>
            <h2>Загрузка игры</h2>
            <div class="saved-games-list" id="saved-games-list">
                <!-- Список сохранений будет здесь -->
            </div>
        </div>
    </div>
    
    <script>
        document.addEventListener('DOMContentLoaded', () => {
            const gameContainer = document.getElementById('game-container');
            const grid = document.getElementById('grid');
            const scoreDisplay = document.getElementById('score');
            const timerDisplay = document.getElementById('timer');
            const newGameBtn = document.getElementById('new-game-btn');
            const saveGameBtn = document.getElementById('save-game-btn');
            const loadGameBtn = document.getElementById('load-game-btn');
            const saveModal = document.getElementById('save-modal');
            const loadModal = document.getElementById('load-modal');
            const saveNameInput = document.getElementById('save-name');
            const confirmSaveBtn = document.getElementById('confirm-save');
            const savedGamesList = document.getElementById('saved-games-list');
            const closeButtons = document.querySelectorAll('.close');
            
            // Размеры клетки и отступы
            const cellSize = 97.5;
            const gapSize = 15;
            const boardPadding = 15;
            
            let board = [];
            let score = 0;
            let isGameOver = false;
            let seconds = 0;
            let timerInterval;
            let saves = {};
            
            // Инициализация игры
            function init() {
                // Загружаем сохранения из localStorage
                loadSaves();
                
                // Назначаем обработчики событий
                setupEventListeners();
                
                // Начинаем новую игру
                initGame();
            }
            
            // Назначаем обработчики событий
            function setupEventListeners() {
                newGameBtn.addEventListener('click', initGame);
                saveGameBtn.addEventListener('click', () => saveModal.style.display = 'block');
                loadGameBtn.addEventListener('click', () => {
                    updateSavedGamesList();
                    loadModal.style.display = 'block';
                });
                confirmSaveBtn.addEventListener('click', saveCurrentGame);
                
                closeButtons.forEach(btn => {
                    btn.addEventListener('click', () => {
                        saveModal.style.display = 'none';
                        loadModal.style.display = 'none';
                    });
                });
                
                // Закрытие модальных окон при клике вне их
                window.addEventListener('click', (event) => {
                    if (event.target === saveModal) {
                        saveModal.style.display = 'none';
                    }
                    if (event.target === loadModal) {
                        loadModal.style.display = 'none';
                    }
                });
                
                // Обработка нажатий клавиш
                document.addEventListener('keydown', handleKeyDown);
            }
            
            // Загрузка сохранений из localStorage
            function loadSaves() {
                const savedGames = localStorage.getItem('2048_saves');
                if (savedGames) {
                    saves = JSON.parse(savedGames);
                } else {
                    saves = {};
                }
            }
            
            // Сохранение списка игр в localStorage
            function saveSaves() {
                localStorage.setItem('2048_saves', JSON.stringify(saves));
            }
            
            // Обновление списка сохранений в модальном окне
            function updateSavedGamesList() {
                savedGamesList.innerHTML = '';
                
                if (Object.keys(saves).length === 0) {
                    savedGamesList.innerHTML = '<p>Нет сохранённых игр</p>';
                    return;
                }
                
                for (const [name, data] of Object.entries(saves)) {
                    const gameElement = document.createElement('div');
                    gameElement.className = 'saved-game';
                    gameElement.innerHTML = `
                        <strong>${name}</strong><br>
                        Счет: ${data.score} | Время: ${formatTime(data.time)}
                    `;
                    gameElement.addEventListener('click', () => {
                        loadGame(name);
                        loadModal.style.display = 'none';
                    });
                    savedGamesList.appendChild(gameElement);
                }
            }
            
            // Сохранение текущей игры
            function saveCurrentGame() {
                const saveName = saveNameInput.value.trim();
                if (!saveName) {
                    alert('Введите название сохранения!');
                    return;
                }
                
                const gameState = {
                    board: board,
                    score: score,
                    time: seconds,
                    date: new Date().toLocaleString()
                };
                
                saves[saveName] = gameState;
                saveSaves();
                saveModal.style.display = 'none';
                saveNameInput.value = '';
                alert(`Игра "${saveName}" сохранена!`);
            }
            
            // Загрузка игры по имени
            function loadGame(name) {
                if (!saves[name]) {
                    alert('Сохранение не найдено!');
                    return;
                }
                
                const gameState = saves[name];
                board = gameState.board;
                score = gameState.score;
                seconds = gameState.time;
                
                scoreDisplay.textContent = score;
                timerDisplay.textContent = formatTime(seconds);
                
                // Удаляем все плитки и сообщение о конце игры
                document.querySelectorAll('.tile').forEach(tile => tile.remove());
                const gameOverElement = document.querySelector('.game-over');
                if (gameOverElement) gameOverElement.remove();
                
                // Обновляем отображение
                updateView();
                
                // Перезапускаем таймер
                clearInterval(timerInterval);
                timerInterval = setInterval(() => {
                    seconds++;
                    timerDisplay.textContent = formatTime(seconds);
                }, 1000);
                
                isGameOver = false;
            }
            
            // Форматирование времени
            function formatTime(seconds) {
                const mins = Math.floor(seconds / 60);
                const secs = seconds % 60;
                return `${mins}:${secs < 10 ? '0' : ''}${secs}`;
            }
            
            // Запуск таймера
            function startTimer() {
                seconds = 0;
                timerDisplay.textContent = formatTime(seconds);
                clearInterval(timerInterval);
                timerInterval = setInterval(() => {
                    seconds++;
                    timerDisplay.textContent = formatTime(seconds);
                }, 1000);
            }
            
            // Инициализация игры
            function initGame() {
                // Очищаем доску
                board = [
                    [0, 0, 0, 0],
                    [0, 0, 0, 0],
                    [0, 0, 0, 0],
                    [0, 0, 0, 0]
                ];
                
                score = 0;
                isGameOver = false;
                scoreDisplay.textContent = score;
                startTimer();
                
                // Удаляем все плитки и сообщение о конце игры
                document.querySelectorAll('.tile').forEach(tile => tile.remove());
                const gameOverElement = document.querySelector('.game-over');
                if (gameOverElement) gameOverElement.remove();
                
                // Добавляем две начальные плитки
                addRandomTile();
                addRandomTile();
                
                // Обновляем отображение
                updateView();
            }
            
            // Добавляем случайную плитку (2 или 4) на свободное место
            function addRandomTile() {
                if (isBoardFull()) return;
                
                let value = Math.random() < 0.9 ? 2 : 4;
                let emptyCells = [];
                
                // Находим все пустые клетки
                for (let row = 0; row < 4; row++) {
                    for (let col = 0; col < 4; col++) {
                        if (board[row][col] === 0) {
                            emptyCells.push({ row, col });
                        }
                    }
                }
                
                // Выбираем случайную пустую клетку
                let randomCell = emptyCells[Math.floor(Math.random() * emptyCells.length)];
                board[randomCell.row][randomCell.col] = value;
            }
            
            // Проверяем, заполнена ли доска
            function isBoardFull() {
                for (let row = 0; row < 4; row++) {
                    for (let col = 0; col < 4; col++) {
                        if (board[row][col] === 0) {
                            return false;
                        }
                    }
                }
                return true;
            }
            
            // Проверяем, возможен ли ход
            function canMove() {
                // Проверяем наличие пустых клеток
                if (!isBoardFull()) return true;
                
                // Проверяем возможные слияния по горизонтали
                for (let row = 0; row < 4; row++) {
                    for (let col = 0; col < 3; col++) {
                        if (board[row][col] === board[row][col + 1]) {
                            return true;
                        }
                    }
                }
                
                // Проверяем возможные слияния по вертикали
                for (let col = 0; col < 4; col++) {
                    for (let row = 0; row < 3; row++) {
                        if (board[row][col] === board[row + 1][col]) {
                            return true;
                        }
                    }
                }
                
                return false;
            }
            
            // Обновляем отображение доски
            function updateView() {
                // Удаляем все плитки
                document.querySelectorAll('.tile').forEach(tile => tile.remove());
                
                // Добавляем плитки на доску
                for (let row = 0; row < 4; row++) {
                    for (let col = 0; col < 4; col++) {
                        if (board[row][col] !== 0) {
                            createTile(row, col, board[row][col]);
                        }
                    }
                }
            }
            
            // Создаем элемент плитки с правильным позиционированием
            function createTile(row, col, value) {
                const tile = document.createElement('div');
                tile.className = `tile tile-${value}`;
                tile.textContent = value;
                
                // Правильно рассчитываем позицию
                const x = col * (cellSize + gapSize) + boardPadding;
                const y = row * (cellSize + gapSize) + boardPadding;
                
                tile.style.left = `${x}px`;
                tile.style.top = `${y}px`;
                
                gameContainer.appendChild(tile);
            }
            
            // Обработка нажатий клавиш
            function handleKeyDown(event) {
                if (isGameOver) return;
                
                let moved = false;
                
                switch (event.key) {
                    case 'ArrowLeft':
                        moved = moveLeft();
                        break;
                    case 'ArrowRight':
                        moved = moveRight();
                        break;
                    case 'ArrowUp':
                        moved = moveUp();
                        break;
                    case 'ArrowDown':
                        moved = moveDown();
                        break;
                    default:
                        return; // Игнорируем другие клавиши
                }
                
                if (moved) {
                    addRandomTile();
                    updateView();
                    scoreDisplay.textContent = score;
                    
                    if (checkWin()) {
                        setTimeout(() => {
                            alert('Поздравляем! Вы достигли 2048!');
                        }, 100);
                    } else if (!canMove()) {
                        handleGameOver();
                    }
                }
            }
            
            // Двигаем плитки влево
            function moveLeft() {
                let moved = false;
                
                for (let row = 0; row < 4; row++) {
                    // Удаляем нули и сливаем одинаковые плитки
                    let newRow = board[row].filter(val => val !== 0);
                    
                    for (let i = 0; i < newRow.length - 1; i++) {
                        if (newRow[i] === newRow[i + 1]) {
                            newRow[i] *= 2;
                            newRow[i + 1] = 0;
                            score += newRow[i];
                            moved = true;
                        }
                    }
                    
                    // Снова удаляем нули
                    newRow = newRow.filter(val => val !== 0);
                    
                    // Заполняем оставшиеся позиции нулями
                    while (newRow.length < 4) {
                        newRow.push(0);
                    }
                    
                    // Проверяем, изменилась ли строка
                    if (!arraysEqual(board[row], newRow)) {
                        moved = true;
                    }
                    
                    board[row] = newRow;
                }
                
                return moved;
            }
            
            // Двигаем плитки вправо
            function moveRight() {
                let moved = false;
                
                for (let row = 0; row < 4; row++) {
                    // Удаляем нули и сливаем одинаковые плитки
                    let newRow = board[row].filter(val => val !== 0);
                    
                    for (let i = newRow.length - 1; i > 0; i--) {
                        if (newRow[i] === newRow[i - 1]) {
                            newRow[i] *= 2;
                            newRow[i - 1] = 0;
                            score += newRow[i];
                            moved = true;
                        }
                    }
                    
                    // Снова удаляем нули
                    newRow = newRow.filter(val => val !== 0);
                    
                    // Заполняем начало нулями
                    while (newRow.length < 4) {
                        newRow.unshift(0);
                    }
                    
                    // Проверяем, изменилась ли строка
                    if (!arraysEqual(board[row], newRow)) {
                        moved = true;
                    }
                    
                    board[row] = newRow;
                }
                
                return moved;
            }
            
            // Двигаем плитки вверх
            function moveUp() {
                let moved = false;
                
                for (let col = 0; col < 4; col++) {
                    // Собираем столбец
                    let column = [board[0][col], board[1][col], board[2][col], board[3][col]];
                    
                    // Удаляем нули и сливаем одинаковые плитки
                    let newColumn = column.filter(val => val !== 0);
                    
                    for (let i = 0; i < newColumn.length - 1; i++) {
                        if (newColumn[i] === newColumn[i + 1]) {
                            newColumn[i] *= 2;
                            newColumn[i + 1] = 0;
                            score += newColumn[i];
                            moved = true;
                        }
                    }
                    
                    // Снова удаляем нули
                    newColumn = newColumn.filter(val => val !== 0);
                    
                    // Заполняем оставшиеся позиции нулями
                    while (newColumn.length < 4) {
                        newColumn.push(0);
                    }
                    
                    // Проверяем, изменился ли столбец
                    if (!arraysEqual(column, newColumn)) {
                        moved = true;
                    }
                    
                    // Обновляем столбец на доске
                    for (let row = 0; row < 4; row++) {
                        board[row][col] = newColumn[row];
                    }
                }
                
                return moved;
            }
            
            // Двигаем плитки вниз
            function moveDown() {
                let moved = false;
                
                for (let col = 0; col < 4; col++) {
                    // Собираем столбец
                    let column = [board[0][col], board[1][col], board[2][col], board[3][col]];
                    
                    // Удаляем нули и сливаем одинаковые плитки
                    let newColumn = column.filter(val => val !== 0);
                    
                    for (let i = newColumn.length - 1; i > 0; i--) {
                        if (newColumn[i] === newColumn[i - 1]) {
                            newColumn[i] *= 2;
                            newColumn[i - 1] = 0;
                            score += newColumn[i];
                            moved = true;
                        }
                    }
                    
                    // Снова удаляем нули
                    newColumn = newColumn.filter(val => val !== 0);
                    
                    // Заполняем начало нулями
                    while (newColumn.length < 4) {
                        newColumn.unshift(0);
                    }
                    
                    // Проверяем, изменился ли столбец
                    if (!arraysEqual(column, newColumn)) {
                        moved = true;
                    }
                    
                    // Обновляем столбец на доске
                    for (let row = 0; row < 4; row++) {
                        board[row][col] = newColumn[row];
                    }
                }
                
                return moved;
            }
            
            // Проверяем, равны ли два массива
            function arraysEqual(a, b) {
                for (let i = 0; i < a.length; i++) {
                    if (a[i] !== b[i]) return false;
                }
                return true;
            }
            
            // Проверяем, достигнута ли победа (плитка 2048)
            function checkWin() {
                for (let row = 0; row < 4; row++) {
                    for (let col = 0; col < 4; col++) {
                        if (board[row][col] === 2048) {
                            return true;
                        }
                    }
                }
                return false;
            }
            
            // Обрабатываем конец игры
            function handleGameOver() {
                isGameOver = true;
                clearInterval(timerInterval);
                
                const gameOverElement = document.createElement('div');
                gameOverElement.className = 'game-over';
                
                const gameOverText = document.createElement('div');
                gameOverText.className = 'game-over-text';
                gameOverText.textContent = 'Игра окончена!';
                
                const restartButton = document.createElement('button');
                restartButton.textContent = 'Новая игра';
                restartButton.addEventListener('click', initGame);
                
                gameOverElement.appendChild(gameOverText);
                gameOverElement.appendChild(restartButton);
                
                gameContainer.appendChild(gameOverElement);
            }
            
            // Запускаем игру
            init();
        });
    </script>
</body>
</html>
