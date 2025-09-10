# csgo-edition
01
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>CS:GO Browser Edition</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            touch-action: manipulation;
            -webkit-tap-highlight-color: transparent;
        }
        
        body {
            background: linear-gradient(135deg, #1a1a2e, #16213e);
            color: white;
            overflow: hidden;
            height: 100vh;
        }
        
        #game-container {
            width: 100%;
            height: 100%;
            display: flex;
            flex-direction: column;
            position: relative;
        }
        
        #menu-screen, #game-screen, #instructions-screen, #shop-screen {
            width: 100%;
            height: 100%;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            transition: opacity 0.5s;
        }
        
        #game-screen, #shop-screen, #instructions-screen {
            display: none;
            position: relative;
        }
        
        .title {
            font-size: 48px;
            margin-bottom: 30px;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
            color: #ffa500;
            text-align: center;
        }
        
        .subtitle {
            font-size: 28px;
            margin-bottom: 20px;
            color: #ffa500;
            text-align: center;
        }
        
        .btn {
            padding: 15px 30px;
            margin: 10px;
            font-size: 18px;
            background: linear-gradient(to bottom, #ff7b00, #ff5500);
            border: none;
            border-radius: 5px;
            color: white;
            cursor: pointer;
            transition: all 0.3s;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
        }
        
        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 12px rgba(0, 0, 0, 0.4);
        }
        
        .btn:active {
            transform: translateY(0);
        }
        
        #game-area {
            width: 100%;
            max-width: 900px;
            height: 55vh;
            background-color: #2c3e50;
            border: 4px solid #ecf0f1;
            border-radius: 8px;
            overflow: hidden;
            position: relative;
            box-shadow: 0 0 20px rgba(0, 0, 0, 0.5);
            background-image: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="100" height="100" viewBox="0 0 100 100"><rect fill="%232c3e50" width="100" height="100"/><path fill="%2334495e" d="M0 0h50v50H0z"/></svg>');
            background-size: 50px 50px;
        }
        
        #player {
            width: 30px;
            height: 30px;
            background-color: #3498db;
            position: absolute;
            bottom: 50px;
            left: 100px;
            border-radius: 50%;
            z-index: 100;
            box-shadow: 0 0 10px rgba(52, 152, 219, 0.8);
        }
        
        .bot {
            width: 30px;
            height: 30px;
            background-color: #e74c3c;
            position: absolute;
            border-radius: 50%;
            z-index: 90;
            box-shadow: 0 0 10px rgba(231, 76, 60, 0.8);
        }
        
        .bullet {
            width: 5px;
            height: 10px;
            background-color: yellow;
            position: absolute;
            z-index: 80;
        }
        
        .grenade {
            width: 15px;
            height: 15px;
            background-color: #27ae60;
            position: absolute;
            border-radius: 50%;
            z-index: 85;
        }
        
        .bomb {
            width: 20px;
            height: 20px;
            background-color: #2c3e50;
            border: 2px solid #7f8c8d;
            position: absolute;
            border-radius: 50%;
            z-index: 85;
        }
        
        .cover {
            position: absolute;
            background-color: #7f8c8d;
            border: 2px solid #34495e;
            z-index: 70;
        }
        
        #hud {
            width: 100%;
            max-width: 900px;
            display: flex;
            justify-content: space-between;
            padding: 10px;
            background-color: rgba(0, 0, 0, 0.7);
            border-radius: 5px;
            margin-top: 10px;
            flex-wrap: wrap;
        }
        
        .hud-section {
            margin: 5px;
            min-width: 120px;
        }
        
        #weapon-selector {
            display: flex;
            margin-top: 10px;
            background-color: rgba(0, 0, 0, 0.7);
            padding: 10px;
            border-radius: 5px;
            flex-wrap: wrap;
            justify-content: center;
            max-width: 900px;
            width: 100%;
        }
        
        .weapon-btn {
            padding: 8px 15px;
            margin: 5px;
            background-color: #34495e;
            border: none;
            border-radius: 3px;
            color: white;
            cursor: pointer;
            transition: all 0.2s;
        }
        
        .weapon-btn.selected {
            background-color: #e67e22;
            transform: scale(1.05);
        }
        
        #instructions-content {
            background-color: rgba(0, 0, 0, 0.7);
            padding: 20px;
            border-radius: 10px;
            max-width: 600px;
            text-align: left;
            overflow-y: auto;
            max-height: 70vh;
        }
        
        #instructions-content p {
            margin: 10px 0;
            line-height: 1.6;
        }
        
        .key {
            display: inline-block;
            padding: 2px 6px;
            background-color: #34495e;
            border-radius: 4px;
            font-weight: bold;
            margin: 0 3px;
        }
        
        .back-btn {
            margin-top: 20px;
        }
        
        .health-bar {
            width: 100px;
            height: 15px;
            background-color: #2c3e50;
            border-radius: 5px;
            overflow: hidden;
        }
        
        .health-fill {
            height: 100%;
            background-color: #2ecc71;
            width: 100%;
        }
        
        #score {
            font-size: 18px;
            font-weight: bold;
        }
        
        #shop-items {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            max-width: 900px;
            margin-top: 20px;
            overflow-y: auto;
            max-height: 60vh;
        }
        
        .shop-item {
            width: 200px;
            padding: 15px;
            margin: 10px;
            background: linear-gradient(135deg, #34495e, #2c3e50);
            border-radius: 8px;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s;
        }
        
        .shop-item:hover {
            transform: translateY(-5px);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
        }
        
        .shop-item h3 {
            color: #ffa500;
            margin-bottom: 10px;
        }
        
        .shop-item p {
            margin: 5px 0;
        }
        
        .price {
            font-weight: bold;
            color: #2ecc71;
        }
        
        #round-info {
            position: absolute;
            top: 20px;
            left: 50%;
            transform: translateX(-50%);
            background-color: rgba(0, 0, 0, 0.7);
            padding: 10px 20px;
            border-radius: 5px;
            font-weight: bold;
            display: none;
            z-index: 200;
        }
        
        #game-over {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background-color: rgba(0, 0, 0, 0.9);
            padding: 30px;
            border-radius: 10px;
            text-align: center;
            display: none;
            z-index: 1000;
            width: 90%;
            max-width: 500px;
        }
        
        #victory {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background-color: rgba(0, 0, 0, 0.9);
            padding: 30px;
            border-radius: 10px;
            text-align: center;
            display: none;
            z-index: 1000;
            width: 90%;
            max-width: 500px;
        }
        
        /* Мобильное управление */
        #mobile-controls {
            position: absolute;
            bottom: 120px;
            left: 0;
            width: 100%;
            display: none;
            justify-content: space-between;
            padding: 0 20px;
            z-index: 300;
        }
        
        #left-control {
            width: 120px;
            height: 120px;
            background-color: rgba(255, 255, 255, 0.2);
            border-radius: 50%;
            position: relative;
            touch-action: none;
        }
        
        #joystick {
            width: 50px;
            height: 50px;
            background-color: rgba(255, 255, 255, 0.5);
            border-radius: 50%;
            position: absolute;
            top: 35px;
            left: 35px;
        }
        
        #right-control {
            width: 120px;
            height: 120px;
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            align-items: center;
        }
        
        .action-btn {
            width: 50px;
            height: 50px;
            background-color: rgba(255, 165, 0, 0.7);
            border-radius: 50%;
            display: flex;
            justify-content: center;
            align-items: center;
            margin: 5px;
            font-size: 20px;
            font-weight: bold;
            user-select: none;
        }
        
        #shoot-btn {
            width: 60px;
            height: 60px;
            background-color: rgba(255, 0, 0, 0.7);
        }
        
        #mobile-weapon-selector {
            position: absolute;
            bottom: 70px;
            left: 0;
            width: 100%;
            display: none;
            justify-content: center;
            padding: 0 10px;
            z-index: 300;
        }
        
        .mobile-weapon-btn {
            padding: 5px 10px;
            margin: 2px;
            background-color: #34495e;
            border: none;
            border-radius: 3px;
            color: white;
            font-size: 12px;
        }
        
        /* Адаптивность */
        @media (max-width: 768px) {
            .title {
                font-size: 36px;
            }
            
            .subtitle {
                font-size: 24px;
            }
            
            .btn {
                padding: 12px 24px;
                font-size: 16px;
            }
            
            #game-area {
                height: 40vh;
            }
            
            #hud {
                flex-direction: column;
                align-items: center;
                padding: 5px;
            }
            
            .hud-section {
                margin: 2px;
                min-width: auto;
                width: 100%;
                text-align: center;
            }
            
            #weapon-selector {
                display: none;
            }
            
            #mobile-controls {
                display: flex;
            }
            
            #mobile-weapon-selector {
                display: flex;
            }
            
            #instructions-content {
                max-height: 60vh;
                padding: 15px;
                font-size: 14px;
            }
            
            #shop-items {
                max-height: 50vh;
            }
            
            .shop-item {
                width: 150px;
                padding: 10px;
                margin: 5px;
            }
            
            .shop-item h3 {
                font-size: 16px;
            }
            
            .shop-item p {
                font-size: 14px;
            }
        }
        
        @media (max-width: 480px) {
            .title {
                font-size: 28px;
            }
            
            #game-area {
                height: 35vh;
            }
            
            #left-control, #right-control {
                width: 100px;
                height: 100px;
            }
            
            #joystick {
                width: 40px;
                height: 40px;
                top: 30px;
                left: 30px;
            }
            
            .action-btn {
                width: 40px;
                height: 40px;
                font-size: 16px;
            }
            
            #shoot-btn {
                width: 50px;
                height: 50px;
            }
            
            .mobile-weapon-btn {
                padding: 3px 6px;
                font-size: 10px;
            }
        }
        
        /* Для очень маленьких экранов */
        @media (max-height: 600px) {
            #game-area {
                height: 30vh;
            }
            
            #mobile-controls {
                bottom: 100px;
            }
        }
    </style>
</head>
<body>
    <div id="game-container">
        <!-- Главное меню -->
        <div id="menu-screen">
            <h1 class="title">CS:GO Browser Edition</h1>
            <button class="btn" onclick="startGame()">Начать игру</button>
            <button class="btn" onclick="showInstructions()">Инструкция</button>
            <p style="margin-top: 20px; text-align: center; padding: 0 10px;">Убивайте врагов, зарабатывайте очки, покупайте лучшее оружие!</p>
        </div>
        
        <!-- Экран игры -->
        <div id="game-screen">
            <div id="round-info">Раунд 1</div>
            
            <div id="game-area">
                <div id="player"></div>
                <!-- Укрытия -->
                <div class="cover" style="width: 100px; height: 40px; left: 200px; bottom: 50px;"></div>
                <div class="cover" style="width: 100px; height: 40px; left: 500px; bottom: 120px;"></div>
                <div class="cover" style="width: 100px; height: 40px; left: 300px; bottom: 200px;"></div>
                <div class="cover" style="width: 100px; height: 40px; left: 700px; bottom: 80px;"></div>
                <div class="cover" style="width: 100px; height: 40px; left: 600px; bottom: 250px;"></div>
                <!-- Боты, пули и гранаты будут добавляться динамически -->
            </div>
            
            <div id="hud">
                <div class="hud-section">
                    <div>Здоровье: <span id="health">100</span></div>
                    <div class="health-bar">
                        <div id="health-bar-fill" class="health-fill"></div>
                    </div>
                </div>
                <div class="hud-section">Оружие: <span id="current-weapon">Pistol</span></div>
                <div class="hud-section">Патроны: <span id="ammo">12</span>/36</div>
                <div class="hud-section">Гранаты: <span id="grenades">2</span></div>
                <div class="hud-section" id="score">Счет: 0</div>
                <div class="hud-section" id="money">Деньги: $1000</div>
                <div class="hud-section">
                    <button class="btn" onclick="openShop()" style="padding: 5px 10px; font-size: 14px;">Магазин (M)</button>
                </div>
            </div>
            
            <div id="weapon-selector">
                <button class="weapon-btn selected" data-weapon="knife">Нож</button>
                <button class="weapon-btn" data-weapon="pistol">Pistol</button>
                <button class="weapon-btn" data-weapon="shotgun">Shotgun</button>
                <button class="weapon-btn" data-weapon="ak47">AK-47</button>
                <button class="weapon-btn" data-weapon="awp">AWP</button>
                <button class="weapon-btn" data-weapon="m4a4">M4A4</button>
                <button class="weapon-btn" onclick="throwGrenade()">Бросить гранату (G)</button>
                <button class="weapon-btn" onclick="plantBomb()">Установить бомбу (B)</button>
            </div>
            
            <!-- Мобильное управление -->
            <div id="mobile-controls">
                <div id="left-control">
                    <div id="joystick"></div>
                </div>
                <div id="right-control">
                    <div class="action-btn" id="shoot-btn" ontouchstart="mobileShoot()">F</div>
                    <div class="action-btn" ontouchstart="mobileThrowGrenade()">G</div>
                    <div class="action-btn" ontouchstart="mobileReload()">R</div>
                    <div class="action-btn" ontouchstart="openShop()">M</div>
                </div>
            </div>
            
            <div id="mobile-weapon-selector">
                <button class="mobile-weapon-btn selected" data-weapon="knife">Нож</button>
                <button class="mobile-weapon-btn" data-weapon="pistol">Pistol</button>
                <button class="mobile-weapon-btn" data-weapon="shotgun">Shotgun</button>
                <button class="mobile-weapon-btn" data-weapon="ak47">AK-47</button>
                <button class="mobile-weapon-btn" data-weapon="awp">AWP</button>
                <button class="mobile-weapon-btn" data-weapon="m4a4">M4A4</button>
            </div>
            
            <div id="game-over">
                <h2>Игра окончена!</h2>
                <p>Ваш счет: <span id="final-score">0</span></p>
                <button class="btn" onclick="restartGame()">Заново</button>
                <button class="btn" onclick="showMenu()">В меню</button>
            </div>
            
            <div id="victory">
                <h2>Победа!</h2>
                <p>Вы уничтожили всех врагов!</p>
                <p>Ваш счет: <span id="victory-score">0</span></p>
                <button class="btn" onclick="nextRound()">Следующий раунд</button>
                <button class="btn" onclick="showMenu()">В меню</button>
            </div>
        </div>
        
        <!-- Магазин -->
        <div id="shop-screen">
            <h2 class="subtitle">Магазин оружия</h2>
            <p>Баланс: $<span id="shop-money">1000</span></p>
            
            <div id="shop-items">
                <div class="shop-item" data-item="pistol" data-price="300">
                    <h3>Pistol</h3>
                    <p>Урон: 20</p>
                    <p>Патроны: 12/36</p>
                    <p class="price">$300</p>
                </div>
                
                <div class="shop-item" data-item="shotgun" data-price="600">
                    <h3>Shotgun</h3>
                    <p>Урон: 40</p>
                    <p>Патроны: 8/24</p>
                    <p class="price">$600</p>
                </div>
                
                <div class="shop-item" data-item="ak47" data-price="1200">
                    <h3>AK-47</h3>
                    <p>Урон: 25</p>
                    <p>Патроны: 30/90</p>
                    <p class="price">$1200</p>
                </div>
                
                <div class="shop-item" data-item="m4a4" data-price="1500">
                    <h3>M4A4</h3>
                    <p>Урон: 23</p>
                    <p>Патроны: 30/90</p>
                    <p class="price">$1500</p>
                </div>
                
                <div class="shop-item" data-item="awp" data-price="2000">
                    <h3>AWP</h3>
                    <p>Урон: 100</p>
                    <p>Патроны: 10/30</p>
                    <p class="price">$2000</p>
                </div>
                
                <div class="shop-item" data-item="grenade" data-price="300">
                    <h3>Обычная граната</h3>
                    <p>Урон: 50-100</p>
                    <p>Радиус: 100px</p>
                    <p class="price">$300</p>
                </div>
                
                <div class="shop-item" data-item="strong_grenade" data-price="600">
                    <h3>Сильная граната</h3>
                    <p>Урон: 100-150</p>
                    <p>Радиус: 150px</p>
                    <p class="price">$600</p>
                </div>
                
                <div class="shop-item" data-item="health" data-price="500">
                    <h3>Аптечка</h3>
                    <p>Восстановление: 50 HP</p>
                    <p class="price">$500</p>
                </div>
            </div>
            
            <button class="btn back-btn" onclick="closeShop()">Вернуться в игру</button>
        </div>
        
        <!-- Экран инструкций -->
        <div id="instructions-screen">
            <h2 class="subtitle">Инструкция по игре</h2>
            <div id="instructions-content">
                <p>Добро пожаловать в CS:GO Browser Edition!</p>
                <p><strong>Управление на ПК:</strong></p>
                <p>Движение: <span class="key">W</span> <span class="key">A</span> <span class="key">S</span> <span class="key">D</span></p>
                <p>Стрельба: <span class="key">ЛКМ</span> или <span class="key">Пробел</span></p>
                <p>Прицеливание: <span class="key">ПКМ</span></p>
                <p>Прыжок: <span class="key">Пробел</span></p>
                <p>Смена оружия: <span class="key">1</span>-<span class="key">6</span></p>
                <p>Бросить гранату: <span class="key">G</span></p>
                <p>Установить бомбу: <span class="key">B</span></p>
                <p>Перезарядка: <span class="key">R</span></p>
                <p>Магазин: <span class="key">M</span></p>
                
                <p><strong>Управление на мобильном:</strong></p>
                <p>Движение: виртуальный джойстик в левой части экрана</p>
                <p>Стрельба: кнопка F в правой части экрана</p>
                <p>Бросить гранату: кнопка G</p>
                <p>Перезарядка: кнопка R</p>
                <p>Магазин: кнопка M</p>
                <p>Смена оружия: панель с кнопками оружия внизу экрана</p>
                
                <p><strong>Цель игры:</strong></p>
                <p>Уничтожьте всех врагов в каждом раунде!</p>
                <p>Зарабатывайте деньги за убийства и покупайте лучшее оружие.</p>
                
                <p><strong>Особенности:</strong></p>
                <p>- Разные виды оружия с уникальными характеристиками</p>
                <p>- Система покупки оружия между раундами</p>
                <p>- Реалистичная физика гранат</p>
                <p>- Враги с ИИ, которые атакуют игрока</p>
                <p>- Укрытия для тактического gameplay</p>
                <p>- Увеличивающаяся сложность с каждым раундом</p>
            </div>
            <button class="btn back-btn" onclick="showMenu()">Назад в меню</button>
        </div>
    </div>

    <script>
        // Элементы игры
        const gameContainer = document.getElementById('game-container');
        const menuScreen = document.getElementById('menu-screen');
        const gameScreen = document.getElementById('game-screen');
        const shopScreen = document.getElementById('shop-screen');
        const instructionsScreen = document.getElementById('instructions-screen');
        const gameArea = document.getElementById('game-area');
        const player = document.getElementById('player');
        const healthDisplay = document.getElementById('health');
        const healthBarFill = document.getElementById('health-bar-fill');
        const ammoDisplay = document.getElementById('ammo');
        const grenadesDisplay = document.getElementById('grenades');
        const scoreDisplay = document.getElementById('score');
        const moneyDisplay = document.getElementById('money');
        const currentWeaponDisplay = document.getElementById('current-weapon');
        const roundInfo = document.getElementById('round-info');
        const gameOverScreen = document.getElementById('game-over');
        const victoryScreen = document.getElementById('victory');
        const finalScoreDisplay = document.getElementById('final-score');
        const victoryScoreDisplay = document.getElementById('victory-score');
        const shopMoneyDisplay = document.getElementById('shop-money');
        const leftControl = document.getElementById('left-control');
        const joystick = document.getElementById('joystick');
        
        // Переменные игры
        let playerX = 100;
        let playerY = 420;
        let playerSpeed = 5;
        let playerHealth = 100;
        let playerAmmo = 12;
        let playerTotalAmmo = 36;
        let playerGrenades = 2;
        let playerStrongGrenades = 0;
        let playerMoney = 1000;
        let score = 0;
        let currentWeapon = 'pistol';
        let roundNumber = 1;
        let bots = [];
        let bullets = [];
        let grenades = [];
        let bombs = [];
        let keys = {};
        let gameInterval;
        let botSpawnInterval;
        let isGameOver = false;
        let isMobile = /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent);
        let joystickActive = false;
        let joystickX = 0;
        let joystickY = 0;
        
        // Оружие и их характеристики
        const weapons = {
            knife: { name: 'Нож', damage: 50, range: 30, ammo: 0, price: 0 },
            pistol: { name: 'Pistol', damage: 20, range: 300, ammo: 12, price: 300 },
            shotgun: { name: 'Shotgun', damage: 40, range: 150, ammo: 8, price: 600 },
            ak47: { name: 'AK-47', damage: 25, range: 400, ammo: 30, price: 1200 },
            awp: { name: 'AWP', damage: 100, range: 500, ammo: 10, price: 2000 },
            m4a4: { name: 'M4A4', damage: 23, range: 400, ammo: 30, price: 1500 }
        };
        
        // Начало игры
        function startGame() {
            menuScreen.style.display = 'none';
            gameScreen.style.display = 'flex';
            
            // Показываем мобильное управление, если это мобильное устройство
            if (isMobile) {
                document.getElementById('mobile-controls').style.display = 'flex';
                document.getElementById('mobile-weapon-selector').style.display = 'flex';
            }
            
            // Сброс состояния игрока
            playerHealth = 100;
            playerMoney = 1000;
            score = 0;
            roundNumber = 1;
            isGameOver = false;
            
            changeWeapon('pistol');
            updateHUD();
            
            // Показ информации о раунде
            roundInfo.textContent = `Раунд ${roundNumber}`;
            roundInfo.style.display = 'block';
            setTimeout(() => {
                roundInfo.style.display = 'none';
            }, 3000);
            
            // Запуск игрового цикла
            if (gameInterval) clearInterval(gameInterval);
            if (botSpawnInterval) clearInterval(botSpawnInterval);
            
            gameInterval = setInterval(updateGame, 1000/60);
            
            // Создание ботов
            createBots();
        }
        
        // Создание ботов для текущего раунда
        function createBots() {
            // Удаляем старых ботов
            bots.forEach(bot => {
                if (bot.element.parentNode) {
                    gameArea.removeChild(bot.element);
                }
            });
            bots = [];
            
            // Создаем новых ботов в зависимости от номера раунда
            const botCount = 3 + roundNumber;
            for (let i = 0; i < botCount; i++) {
                spawnBot();
            }
        }
        
        // Создание одного бота
        function spawnBot() {
            const bot = document.createElement('div');
            bot.className = 'bot';
            
            // Размещаем ботов в разных местах
            const side = Math.random() > 0.5 ? 'left' : 'right';
            const x = side === 'left' ? Math.random() * 300 + 50 : Math.random() * 300 + 550;
            const y = Math.random() * 300 + 50;
            
            bot.style.left = x + 'px';
            bot.style.top = y + 'px';
            
            gameArea.appendChild(bot);
            
            // Увеличиваем здоровье ботов с каждым раундом
            const health = 80 + (roundNumber * 10);
            // Увеличиваем скорость ботов с каждым раундом
            const speed = 2 + Math.random() * 2 + (roundNumber * 0.2);
            // Увеличиваем урон ботов с каждым раундом
            const damage = 5 + roundNumber;
            // Увеличиваем частоту атаки ботов с каждым раундом
            const attackRate = 0.01 + (roundNumber * 0.003);
            
            bots.push({
                element: bot,
                x: x,
                y: y,
                health: health,
                maxHealth: health,
                speed: speed,
                attackRate: attackRate,
                damage: damage
            });
        }
        
        // Обновление игры
        function updateGame() {
            if (isGameOver) return;
            
            movePlayer();
            moveBullets();
            moveBots();
            moveGrenades();
            checkCollisions();
            
            // Проверка на победу
            if (bots.length === 0) {
                victory();
            }
        }
        
        // Движение игрока
        function movePlayer() {
            // Управление с клавиатуры
            if (keys['ArrowLeft'] || keys['KeyA']) {
                playerX = Math.max(0, playerX - playerSpeed);
            }
            if (keys['ArrowRight'] || keys['KeyD']) {
                playerX = Math.min(gameArea.offsetWidth - 30, playerX + playerSpeed);
            }
            if (keys['ArrowUp'] || keys['KeyW']) {
                playerY = Math.max(0, playerY - playerSpeed);
            }
            if (keys['ArrowDown'] || keys['KeyS']) {
                playerY = Math.min(gameArea.offsetHeight - 30, playerY + playerSpeed);
            }
            
            // Управление с джойстика на мобильных устройствах
            if (isMobile && joystickActive) {
                playerX = Math.max(0, Math.min(gameArea.offsetWidth - 30, playerX + joystickX * playerSpeed));
                playerY = Math.max(0, Math.min(gameArea.offsetHeight - 30, playerY + joystickY * playerSpeed));
            }
            
            player.style.left = playerX + 'px';
            player.style.top = playerY + 'px';
        }
        
        // Движение пуль
        function moveBullets() {
            for (let i = bullets.length - 1; i >= 0; i--) {
                const bullet = bullets[i];
                bullet.y -= bullet.speedY;
                bullet.x += bullet.speedX;
                
                if (bullet.y < 0 || bullet.y > gameArea.offsetHeight || bullet.x < 0 || bullet.x > gameArea.offsetWidth) {
                    gameArea.removeChild(bullet.element);
                    bullets.splice(i, 1);
                } else {
                    bullet.element.style.top = bullet.y + 'px';
                    bullet.element.style.left = bullet.x + 'px';
                }
            }
        }
        
        // Движение ботов
        function moveBots() {
            for (let i = 0; i < bots.length; i++) {
                const bot = bots[i];
                
                // Движение к игроку
                const dx = playerX - bot.x;
                const dy = playerY - bot.y;
                const distance = Math.sqrt(dx * dx + dy * dy);
                
                // Боты становятся агрессивнее с каждым раундом
                const aggression = Math.min(0.9, 0.3 + (roundNumber * 0.05));
                
                if (distance > 150 || Math.random() < aggression) {
                    // Двигаемся к игроку
                    bot.x += (dx / distance) * bot.speed;
                    bot.y += (dy / distance) * bot.speed;
                } else if (distance < 100) {
                    // Отходим от игрока
                    bot.x -= (dx / distance) * bot.speed * 0.5;
                    bot.y -= (dy / distance) * bot.speed * 0.5;
                }
                
                // Ограничиваем позицию бота в пределах игровой области
                bot.x = Math.max(0, Math.min(gameArea.offsetWidth - 30, bot.x));
                bot.y = Math.max(0, Math.min(gameArea.offsetHeight - 30, bot.y));
                
                bot.element.style.left = bot.x + 'px';
                bot.element.style.top = bot.y + 'px';
                
                // Боты стреляют с разной частотой в зависимости от раунда
                if (Math.random() < bot.attackRate) {
                    shoot(bot.x + 15, bot.y, true, bot.damage);
                }
            }
        }
        
        // Движение гранат
        function moveGrenades() {
            for (let i = grenades.length - 1; i >= 0; i--) {
                const grenade = grenades[i];
                grenade.y += grenade.speedY;
                grenade.x += grenade.speedX;
                grenade.speedY += 0.2; // Гравитация
                
                if (grenade.y > gameArea.offsetHeight - 15) {
                    // Взрыв гранаты
                    explodeGrenade(grenade.x, grenade.y, grenade.isStrong);
                    gameArea.removeChild(grenade.element);
                    grenades.splice(i, 1);
                } else {
                    grenade.element.style.top = grenade.y + 'px';
                    grenade.element.style.left = grenade.x + 'px';
                }
            }
        }
        
        // Взрыв гранаты
        function explodeGrenade(x, y, isStrong = false) {
            const radius = isStrong ? 150 : 100;
            const damage = isStrong ? 100 : 50;
            
            // Находим всех ботов в радиусе взрыва
            for (let i = 0; i < bots.length; i++) {
                const bot = bots[i];
                const dx = bot.x - x;
                const dy = bot.y - y;
                const distance = Math.sqrt(dx * dx + dy * dy);
                
                if (distance < radius) {
                    // Урон уменьшается с расстоянием
                    const actualDamage = Math.max(damage * 0.3, damage * (1 - distance / radius));
                    bot.health -= actualDamage;
                    
                    if (bot.health <= 0) {
                        gameArea.removeChild(bot.element);
                        bots.splice(i, 1);
                        i--;
                        
                        // Награда за убийство
                        score += 100;
                        playerMoney += 200;
                        updateHUD();
                    }
                }
            }
            
            // Проверяем игрока
            const dx = playerX - x;
            const dy = playerY - y;
            const distance = Math.sqrt(dx * dx + dy * dy);
            
            if (distance < radius) {
                const actualDamage = Math.max(damage * 0.3, damage * (1 - distance / radius));
                playerHealth -= actualDamage;
                updateHUD();
                
                if (playerHealth <= 0) {
                    gameOver();
                }
            }
            
            // Визуальный эффект взрыва
            const explosion = document.createElement('div');
            explosion.style.position = 'absolute';
            explosion.style.left = (x - radius/2) + 'px';
            explosion.style.top = (y - radius/2) + 'px';
            explosion.style.width = radius + 'px';
            explosion.style.height = radius + 'px';
            explosion.style.backgroundColor = isStrong ? 'rgba(255, 50, 0, 0.5)' : 'rgba(255, 165, 0, 0.5)';
            explosion.style.borderRadius = '50%';
            explosion.style.zIndex = '95';
            gameArea.appendChild(explosion);
            
            setTimeout(() => {
                gameArea.removeChild(explosion);
            }, 500);
        }
        
        // Проверка столкновений
        function checkCollisions() {
            // Проверка столкновений пуль с ботами
            for (let i = bullets.length - 1; i >= 0; i--) {
                const bullet = bullets[i];
                
                for (let j = 0; j < bots.length; j++) {
                    const bot = bots[j];
                    
                    const dx = bullet.x - bot.x;
                    const dy = bullet.y - bot.y;
                    const distance = Math.sqrt(dx * dx + dy * dy);
                    
                    if (distance < 20) {
                        // Попадание в бота
                        bot.health -= bullet.damage;
                        
                        if (bot.health <= 0) {
                            gameArea.removeChild(bot.element);
                            bots.splice(j, 1);
                            
                            // Награда за убийство
                            score += 100;
                            playerMoney += 200;
                            updateHUD();
                        }
                        
                        gameArea.removeChild(bullet.element);
                        bullets.splice(i, 1);
                        break;
                    }
                }
            }
            
            // Проверка столкновений пуль с игроком
            for (let i = bullets.length - 1; i >= 0; i--) {
                const bullet = bullets[i];
                
                if (bullet.isEnemy) {
                    const dx = bullet.x - playerX;
                    const dy = bullet.y - playerY;
                    const distance = Math.sqrt(dx * dx + dy * dy);
                    
                    if (distance < 20) {
                        // Попадание в игрока
                        playerHealth -= bullet.damage;
                        updateHUD();
                        
                        if (playerHealth <= 0) {
                            gameOver();
                        }
                        
                        gameArea.removeChild(bullet.element);
                        bullets.splice(i, 1);
                    }
                }
            }
        }
        
        // Стрельба
        function shoot(x, y, isEnemy = false, damage = 10) {
            if (!isEnemy && playerAmmo <= 0) {
                return;
            }
            
            if (!isEnemy) {
                playerAmmo--;
                updateHUD();
            }
            
            const bullet = document.createElement('div');
            bullet.className = 'bullet';
            bullet.style.left = x + 'px';
            bullet.style.top = y + 'px';
            
            gameArea.appendChild(bullet);
            
            // Направление пули
            let speedX = 0;
            let speedY = isEnemy ? 5 : -5;
            
            // Если это враг, пуля летит в сторону игрока
            if (isEnemy) {
                const dx = playerX - x;
                const dy = playerY - y;
                const distance = Math.sqrt(dx * dx + dy * dy);
                
                speedX = (dx / distance) * 5;
                speedY = (dy / distance) * 5;
            }
            
            bullets.push({
                element: bullet,
                x: x,
                y: y,
                speedX: speedX,
                speedY: speedY,
                damage: isEnemy ? damage : weapons[currentWeapon].damage,
                isEnemy: isEnemy
            });
        }
        
        // Бросок гранаты
        function throwGrenade(isStrong = false) {
            if ((isStrong && playerStrongGrenades <= 0) || (!isStrong && playerGrenades <= 0)) return;
            
            if (isStrong) {
                playerStrongGrenades--;
            } else {
                playerGrenades--;
            }
            
            updateHUD();
            
            const grenade = document.createElement('div');
            grenade.className = 'grenade';
            grenade.style.left = playerX + 'px';
            grenade.style.top = playerY + 'px';
            
            if (isStrong) {
                grenade.style.backgroundColor = '#ff4500';
            }
            
            gameArea.appendChild(grenade);
            
            grenades.push({
                element: grenade,
                x: playerX,
                y: playerY,
                speedX: (Math.random() - 0.5) * 5,
                speedY: -10,
                isStrong: isStrong
            });
        }
        
        // Установка бомбы
        function plantBomb() {
            const bomb = document.createElement('div');
            bomb.className = 'bomb';
            bomb.style.left = playerX + 'px';
            bomb.style.top = playerY + 'px';
            
            gameArea.appendChild(bomb);
            
            bombs.push({
                element: bomb,
                x: playerX,
                y: playerY,
                timer: 10 // 10 секунд до взрыва
            });
            
            // Таймер бомбы
            const bombInterval = setInterval(() => {
                if (bombs.length === 0) {
                    clearInterval(bombInterval);
                    return;
                }
                
                const currentBomb = bombs[bombs.length - 1];
                currentBomb.timer--;
                
                if (currentBomb.timer <= 0) {
                    clearInterval(bombInterval);
                    explodeBomb(currentBomb);
                }
            }, 1000);
        }
        
        // Взрыв бомбы
        function explodeBomb(bomb) {
            // Находим всех ботов в радиусе взрыва
            for (let i = 0; i < bots.length; i++) {
                const bot = bots[i];
                const dx = bot.x - bomb.x;
                const dy = bot.y - bomb.y;
                const distance = Math.sqrt(dx * dx + dy * dy);
                
                if (distance < 150) {
                    gameArea.removeChild(bot.element);
                    bots.splice(i, 1);
                    i--;
                    score += 200;
                    playerMoney += 300;
                    updateHUD();
                }
            }
            
            // Визуальный эффект взрыва
            const explosion = document.createElement('div');
            explosion.style.position = 'absolute';
            explosion.style.left = (bomb.x - 75) + 'px';
            explosion.style.top = (bomb.y - 75) + 'px';
            explosion.style.width = '150px';
            explosion.style.height = '150px';
            explosion.style.backgroundColor = 'rgba(255, 0, 0, 0.5)';
            explosion.style.borderRadius = '50%';
            explosion.style.zIndex = '95';
            gameArea.appendChild(explosion);
            
            gameArea.removeChild(bomb.element);
            bombs = [];
            
            setTimeout(() => {
                gameArea.removeChild(explosion);
            }, 1000);
        }
        
        // Смена оружия
        function changeWeapon(weapon) {
            currentWeapon = weapon;
            playerAmmo = weapons[weapon].ammo;
            playerTotalAmmo = weapons[weapon].ammo * 3;
            currentWeaponDisplay.textContent = weapons[weapon].name;
            updateHUD();
            
            // Обновляем выделение кнопок оружия
            document.querySelectorAll('.weapon-btn').forEach(btn => {
                btn.classList.remove('selected');
            });
            document.querySelectorAll('.mobile-weapon-btn').forEach(btn => {
                btn.classList.remove('selected');
            });
            document.querySelector(`[data-weapon="${weapon}"]`).classList.add('selected');
        }
        
        // Покупка предмета в магазине
        function buyItem(item, price) {
            if (playerMoney < price) {
                alert('Недостаточно денег!');
                return;
            }
            
            playerMoney -= price;
            
            switch(item) {
                case 'pistol':
                case 'shotgun':
                case 'ak47':
                case 'awp':
                case 'm4a4':
                    changeWeapon(item);
                    break;
                case 'grenade':
                    playerGrenades += 1;
                    break;
                case 'strong_grenade':
                    playerStrongGrenades += 1;
                    break;
                case 'health':
                    playerHealth = Math.min(100, playerHealth + 50);
                    break;
            }
            
            updateHUD();
            shopMoneyDisplay.textContent = playerMoney;
            alert(`Вы купили ${item === 'strong_grenade' ? 'сильную гранату' : weapons[item] ? weapons[item].name : 'аптечку'}`);
        }
        
        // Обновление HUD
        function updateHUD() {
            healthDisplay.textContent = playerHealth;
            healthBarFill.style.width = playerHealth + '%';
            ammoDisplay.textContent = `${playerAmmo}/${playerTotalAmmo}`;
            grenadesDisplay.textContent = `${playerGrenades} / ${playerStrongGrenades}`;
            scoreDisplay.textContent = `Счет: ${score}`;
            moneyDisplay.textContent = `Деньги: $${playerMoney}`;
            
            if (playerHealth <= 30) {
                healthBarFill.style.backgroundColor = '#e74c3c';
            } else if (playerHealth <= 60) {
                healthBarFill.style.backgroundColor = '#f39c12';
            } else {
                healthBarFill.style.backgroundColor = '#2ecc71';
            }
        }
        
        // Конец игры
        function gameOver() {
            isGameOver = true;
            clearInterval(gameInterval);
            clearInterval(botSpawnInterval);
            
            finalScoreDisplay.textContent = score;
            gameOverScreen.style.display = 'block';
        }
        
        // Победа в раунде
        function victory() {
            isGameOver = true;
            clearInterval(gameInterval);
            clearInterval(botSpawnInterval);
            
            victoryScoreDisplay.textContent = score;
            victoryScreen.style.display = 'block';
        }
        
        // Следующий раунд
        function nextRound() {
            roundNumber++;
            isGameOver = false;
            victoryScreen.style.display = 'none';
            
            // Награда за прохождение раунда
            playerMoney += 1000;
            playerHealth = 100;
            
            // Показ информации о раунде
            roundInfo.textContent = `Раунд ${roundNumber}`;
            roundInfo.style.display = 'block';
            setTimeout(() => {
                roundInfo.style.display = 'none';
            }, 3000);
            
            updateHUD();
            
            // Создание ботов для нового раунда
            createBots();
            
            // Запуск игрового цикла
            gameInterval = setInterval(updateGame, 1000/60);
        }
        
        // Перезапуск игры
        function restartGame() {
            gameOverScreen.style.display = 'none';
            startGame();
        }
        
        // Открытие магазина
        function openShop() {
            gameScreen.style.display = 'none';
            shopScreen.style.display = 'flex';
            shopMoneyDisplay.textContent = playerMoney;
        }
        
        // Закрытие магазина
        function closeShop() {
            shopScreen.style.display = 'none';
            gameScreen.style.display = 'flex';
        }
        
        // Показать инструкции
        function showInstructions() {
            menuScreen.style.display = 'none';
            instructionsScreen.style.display = 'flex';
        }
        
        // Показать меню
        function showMenu() {
            instructionsScreen.style.display = 'none';
            gameScreen.style.display = 'none';
            shopScreen.style.display = 'none';
            gameOverScreen.style.display = 'none';
            victoryScreen.style.display = 'none';
            menuScreen.style.display = 'flex';
            
            // Очистка игры
            clearInterval(gameInterval);
            clearInterval(botSpawnInterval);
            
            bots.forEach(bot => {
                if (bot.element.parentNode) {
                    gameArea.removeChild(bot.element);
                }
            });
            
            bullets.forEach(bullet => {
                if (bullet.element.parentNode) {
                    gameArea.removeChild(bullet.element);
                }
            });
            
            grenades.forEach(grenade => {
                if (grenade.element.parentNode) {
                    gameArea.removeChild(grenade.element);
                }
            });
            
            bombs.forEach(bomb => {
                if (bomb.element.parentNode) {
                    gameArea.removeChild(bomb.element);
                }
            });
            
            bots = [];
            bullets = [];
            grenades = [];
            bombs = [];
        }
        
        // Мобильная стрельба
        function mobileShoot() {
            if (gameScreen.style.display === 'flex' && !isGameOver) {
                shoot(playerX + 15, playerY);
            }
        }
        
        // Мобильный бросок гранаты
        function mobileThrowGrenade() {
            if (gameScreen.style.display === 'flex' && !isGameOver) {
                throwGrenade();
            }
        }
        
        // Мобильная перезарядка
        function mobileReload() {
            if (playerTotalAmmo > 0 && !isGameOver) {
                const neededAmmo = weapons[currentWeapon].ammo - playerAmmo;
                const ammoToReload = Math.min(neededAmmo, playerTotalAmmo);
                
                playerAmmo += ammoToReload;
                playerTotalAmmo -= ammoToReload;
                updateHUD();
            }
        }
        
        // Обработка нажатий клавиш
        document.addEventListener('keydown', (e) => {
            keys[e.code] = true;
            
            // Стрельба на пробел
            if (e.code === 'Space' && gameScreen.style.display === 'flex' && !isGameOver) {
                shoot(playerX + 15, playerY);
            }
            
            // Смена оружия цифрами
            if (e.code === 'Digit1') changeWeapon('knife');
            if (e.code === 'Digit2') changeWeapon('pistol');
            if (e.code === 'Digit3') changeWeapon('shotgun');
            if (e.code === 'Digit4') changeWeapon('ak47');
            if (e.code === 'Digit5') changeWeapon('awp');
            if (e.code === 'Digit6') changeWeapon('m4a4');
            
            // Бросок гранаты на G
            if (e.code === 'KeyG' && !isGameOver) throwGrenade();
            
            // Сильная граната на H
            if (e.code === 'KeyH' && !isGameOver) throwGrenade(true);
            
            // Установка бомбы на B
            if (e.code === 'KeyB' && !isGameOver) plantBomb();
            
            // Перезарядка на R
            if (e.code === 'KeyR' && playerTotalAmmo > 0 && !isGameOver) {
                const neededAmmo = weapons[currentWeapon].ammo - playerAmmo;
                const ammoToReload = Math.min(neededAmmo, playerTotalAmmo);
                
                playerAmmo += ammoToReload;
                playerTotalAmmo -= ammoToReload;
                updateHUD();
            }
            
            // Магазин на M
            if (e.code === 'KeyM' && gameScreen.style.display === 'flex' && !isGameOver) {
                openShop();
            }
        });
        
        document.addEventListener('keyup', (e) => {
            keys[e.code] = false;
        });
        
        // Обработка кликов по кнопкам оружия
        document.querySelectorAll('[data-weapon]').forEach(btn => {
            btn.addEventListener('click', () => {
                if (!isGameOver) changeWeapon(btn.dataset.weapon);
            });
        });
        
        // Обработка кликов по предметам в магазине
        document.querySelectorAll('.shop-item').forEach(item => {
            item.addEventListener('click', () => {
                buyItem(item.dataset.item, parseInt(item.dataset.price));
            });
        });
        
        // Стрельба по клику мыши
        gameArea.addEventListener('click', () => {
            if (gameScreen.style.display === 'flex' && !isGameOver) {
                shoot(playerX + 15, playerY);
            }
        });
        
        // Мобильное управление - джойстик
        if (isMobile) {
            let touchId = null;
            
            leftControl.addEventListener('touchstart', (e) => {
                e.preventDefault();
                const touch = e.touches[0];
                touchId = touch.identifier;
                joystickActive = true;
                
                const rect = leftControl.getBoundingClientRect();
                const centerX = rect.left + rect.width / 2;
                const centerY = rect.top + rect.height / 2;
                
                joystickX = (touch.clientX - centerX) / (rect.width / 2);
                joystickY = (touch.clientY - centerY) / (rect.height / 2);
                
                // Ограничиваем значения джойстика
                const length = Math.sqrt(joystickX * joystickX + joystickY * joystickY);
                if (length > 1) {
                    joystickX /= length;
                    joystickY /= length;
                }
                
                // Перемещаем визуальный джойстик
                joystick.style.left = (35 + joystickX * 35) + 'px';
                joystick.style.top = (35 + joystickY * 35) + 'px';
            });
            
            leftControl.addEventListener('touchmove', (e) => {
                e.preventDefault();
                if (!touchId) return;
                
                // Находим наш touch по identifier
                let touch = null;
                for (let i = 0; i < e.touches.length; i++) {
                    if (e.touches[i].identifier === touchId) {
                        touch = e.touches[i];
                        break;
                    }
                }
                
                if (!touch) return;
                
                const rect = leftControl.getBoundingClientRect();
                const centerX = rect.left + rect.width / 2;
                const centerY = rect.top + rect.height / 2;
                
                joystickX = (touch.clientX - centerX) / (rect.width / 2);
                joystickY = (touch.clientY - centerY) / (rect.height / 2);
                
                // Ограничиваем значения джойстика
                const length = Math.sqrt(joystickX * joystickX + joystickY * joystickY);
                if (length > 1) {
                    joystickX /= length;
                    joystickY /= length;
                }
                
                // Перемещаем визуальный джойстик
                joystick.style.left = (35 + joystickX * 35) + 'px';
                joystick.style.top = (35 + joystickY * 35) + 'px';
            });
            
            leftControl.addEventListener('touchend', (e) => {
                e.preventDefault();
                touchId = null;
                joystickActive = false;
                joystickX = 0;
                joystickY = 0;
                
                // Возвращаем джойстик в центр
                joystick.style.left = '35px';
                joystick.style.top = '35px';
            });
            
            leftControl.addEventListener('touchcancel', (e) => {
                e.preventDefault();
                touchId = null;
                joystickActive = false;
                joystickX = 0;
                joystickY = 0;
                
                // Возвращаем джойстик в центр
                joystick.style.left = '35px';
                joystick.style.top = '35px';
            });
        }
    </script>
</body>
</html>
