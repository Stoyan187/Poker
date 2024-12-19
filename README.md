<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>♠️♥️ Poker ♦️♣️</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            background-color: #2c2c2c;
            margin: 0;
            padding: 0;
            color: white;
            display: flex;
            justify-content: center;
            padding-top: 30px;
        }
        h1 {
            color: #f1f1f1;
            width: 100%;
        }
        .main-content {
            width: 60%;
            padding: 20px;
        }
        .player {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            margin: 10px auto;
            padding: 10px;
            width: 80%;
            max-width: 400px;
            background: #333;
            border: 1px solid #777;
            border-radius: 8px;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
            position: relative;
        }
        .player .rank {
            font-size: 20px;
            font-weight: bold;
            color: #f1f1f1;
            position: absolute;
            top: 10px;
            right: 10px;
        }
        .player span {
            font-size: 18px;
            margin: 5px 0;
        }
        .player .name {
            font-size: 22px;
            font-weight: bold;
        }
        .player button,
        .chip-buttons button {
            background-color: #8b0000;
            color: white;
            border: none;
            border-radius: 50%;
            padding: 12px 20px;
            font-size: 16px;
            cursor: pointer;
            margin: 5px;
            transition: background-color 0.3s;
        }
        .player button:hover,
        .chip-buttons button:hover {
            background-color: #b22222;
        }
        .player button:active,
        .chip-buttons button:active {
            background-color: #800000;
        }
        .chip-buttons {
            display: flex;
            justify-content: center;
            flex-wrap: wrap;
            margin-top: 20px;
        }
        .add-player-button {
            background-color: #555;
            padding: 10px 20px;
            color: white;
            font-size: 16px;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            margin-top: 20px;
        }
        .add-player-button:hover {
            background-color: #444;
        }
    </style>
</head>
<body>
    <div class="main-content">
        <h1>Poker Chip Counter</h1>
        <div id="players"></div>
        <button class="add-player-button" onclick="addPlayer()">Add Player</button>
    </div>

    <script>
        let players = [];

        // Функция за рендериране на играчите в лявата част (само бутоните за добавяне и премахване на чипове)
        function renderPlayers() {
            const playersDiv = document.getElementById('players');
            playersDiv.innerHTML = ''; // Изчистваме текущия списък

            // Преизчисляваме позициите на играчите според техните чипове
            players.forEach((player, index) => {
                const playerDiv = document.createElement('div');
                playerDiv.className = 'player';
                playerDiv.setAttribute('data-index', index);  // Добавяме индекс на всеки играч

                // Вкарваме в картата името, броя на чиповете и бутони за добавяне и премахване на чипове
                playerDiv.innerHTML = `
                    <span class="rank" id="rank-${index}">${getRank(player)}</span> <!-- Позицията в класацията -->
                    <span class="name">${player.name}</span>
                    <span>Chips: <b>${player.chips}</b></span>
                    <div>
                        <button onclick="updateChips(${index}, -5)">-5</button>
                        <button onclick="updateChips(${index}, -10)">-10</button>
                        <button onclick="updateChips(${index}, -15)">-15</button>
                        <button onclick="updateChips(${index}, -50)">-50</button>
                        <button onclick="updateChips(${index}, -100)">-100</button>
                    </div>
                    <div class="chip-buttons">
                        <button onclick="addChips(${index}, 5)">+5</button>
                        <button onclick="addChips(${index}, 10)">+10</button>
                        <button onclick="addChips(${index}, 15)">+15</button>
                        <button onclick="addChips(${index}, 50)">+50</button>
                        <button onclick="addChips(${index}, 100)">+100</button>
                    </div>
                `;
                playersDiv.appendChild(playerDiv);
            });
        }

        // Функция за добавяне на нов играч
        function addPlayer() {
            const name = prompt('Enter player name:');
            if (name) {
                players.push({ name, chips: 0 });
                renderPlayers();  // Рендерираме лявата част
            }
        }

        // Функция за обновяване на чиповете на играч
        function updateChips(index, change) {
            players[index].chips += change;
            renderPlayers();  // Актуализираме лявата част с новия ред
        }

        // Функция за добавяне на чипове на играч
        function addChips(index, amount) {
            players[index].chips += amount;
            renderPlayers();  // Актуализираме лявата част с новия ред
        }

        // Функция за получаване на ранга на играча според броя на чиповете
        function getRank(player) {
            // Сортираме играчите по брой чипове, за да можем да изчислим ранга
            const sortedPlayers = [...players].sort((a, b) => b.chips - a.chips);
            return sortedPlayers.findIndex(p => p.name === player.name) + 1;
        }

        // Функция за актуализиране на ранга в картата на играча
        function updateRank() {
            players.forEach((player, index) => {
                const rankElement = document.getElementById(`rank-${index}`);
                rankElement.innerText = getRank(player);
            });
        }

        // След всяка промяна на чипове ще обновяваме ранга
        function renderPlayersWithRankUpdate() {
            renderPlayers();
            updateRank(); // Актуализираме ранга на играчите след всяка промяна
        }

        // Начално рендериране
        renderPlayers();
    </script>
</body>
</html>
