<template>
  <div class="app-container">
    <header>
      <h1>🎮 Portal de Minijuegos Retro</h1>
      <p>¡Los juegos clásicos de siempre!</p>
    </header>

    <main>
      <!-- Catálogo de juegos -->
      <section v-if="!activeLocalGame">
        <div class="controls">
          <input
            v-model="searchQuery"
            type="text"
            placeholder="🔎 Buscar un juego..."
            class="search-bar"
          />
          <button @click="toggleSortOrder" class="sort-btn">
            Ordenar por año: {{ sortOrder === 'asc' ? '↑' : '↓' }}
          </button>
        </div>

        <div class="games-grid">
          <div v-for="game in displayedGames" :key="game.id" class="game-card">
            <img :src="game.image" :alt="game.name" class="game-img" />
            <div class="game-info">
              <h3>{{ game.name }} <small>({{ game.year }})</small></h3>
              <p>{{ game.description }}</p>
              <button
                class="play-btn"
                :disabled="!game.isLocal"
                @click="playGame(game)"
              >
                {{ game.isLocal ? '▶ Jugar ahora' : 'Próximamente' }}
              </button>
            </div>
          </div>
          <p v-if="displayedGames.length === 0" class="no-results">
            No se encontraron juegos con esa búsqueda.
          </p>
        </div>
      </section>

      <!-- Único juego disponible: tres en raya -->
      <section v-else class="minigame-section">
        <button @click="closeLocalGame" class="back-btn">← Volver al catálogo</button>
        <h2>❌ Tres en Raya ⭕</h2>
        <p class="subtitle">¡Consigue tres símbolos en línea para ganar!</p>

        <div class="tictactoe-board">
          <button
            v-for="(cell, index) in board"
            :key="index"
            class="cell"
            :class="{ 'x-cell': cell === 'X', 'o-cell': cell === 'O' }"
            @click="makeMove(index)"
          >{{ cell }}</button>
        </div>

        <div class="game-status">
          <p v-if="winner && winner !== 'Empate'" class="win-text">🎉 ¡Ganador: {{ winner }}!</p>
          <p v-else-if="winner === 'Empate'" class="tie-text">¡Es un empate!</p>
          <p v-else>Turno del jugador: <strong>{{ xIsNext ? 'X' : 'O' }}</strong></p>
          <button @click="resetGame" class="reset-btn">↻ Reiniciar partida</button>
        </div>
      </section>
    </main>

    <footer>
      <h3>Palabras clave en común:</h3>
      <div class="keywords-list">
        <span v-if="commonKeywords.length === 0">No hay palabras clave en común.</span>
        <span v-for="word in commonKeywords" :key="word" class="keyword-tag">{{ word }}</span>
      </div>
    </footer>
  </div>
</template>

<script setup>

import { ref, computed } from 'vue';

//  Datos del portal de juegos

const searchQuery = ref('');

const sortOrder = ref('desc');

const activeLocalGame = ref(false); // Controla si se muestra el juego local o el catálogo

const games = ref([

  {

    id: 1,

    name: "Tres-en-Raya",

    year: 1300,

    description: "El clásico juego de mesa en la pantalla. Un divertido desafío mental para alinear tres piezas y ganar puntos.",

    image: "/img/3enraya.png",

    isLocal: true, // Identificador de que este juego se abre aquí

    url: null

  },

  {

    id: 2,

    name: "Ahorcado",

    year: 1894,

    description: "El clásico juego de adivinar palabras en la pantalla. Un divertido desafío para evitar la horca y sumar puntos.",

    image: "/img/ahorcado.png",

    isLocal: false,

    url: "https://poki.com/es/g/hangman"

  },

  {

    id: 3,

    name: "Memory Cards",

    year: 1959,

    description: "El clásico juego de memoria en la pantalla. Un divertido desafío de encontrar parejas para obtener puntos.",

    image: "/img/memorygame.png",

    isLocal: false,

    url: "https://www.memozor.com/"

  },

  {

    id: 4,

    name: "Dinosaur Game",

    year: 2014,

    description: "El clásico juego del dinosaurio en la pantalla. Un divertido desafío de saltar obstáculos y conseguir puntos.",

    image: "/img/dinosaur.png",

    isLocal: false,

    url: "https://chromedino.com/"

  },

  {

    id: 5,

    name: "Flappy Bird",

    year: 2013,

    description: "El clásico juego del pájaro en la pantalla. Un divertido desafío de esquivar tuberías para hacer puntos.",

    image: "/img/flappybird.png",

    isLocal: false,

    url: "https://flappybird.io/"

  },

  {

    id: 6,

    name: "Snake",

    year: 1976,

    description: "El clásico juego de la serpiente en la pantalla. Un divertido desafío de comer manzanas y ganar puntos.",

    image: "/img/snake.png",

    isLocal: false,

    url: "https://playsnake.org/"

  },

  {

    id: 7,

    name: "Pac-Man",

    year: 1980,

    description: "El clásico juego del comecocos en la pantalla. Un divertido desafío de escapar de fantasmas para sumar puntos.",

    image: "/img/pacman.png",

    isLocal: false,

    url: "https://freepacman.org/"

  },

  {

    id: 8,

    name: "Pong",

    year: 1972,

    description: "El clásico juego de tenis en la pantalla. Un divertido desafío de devolver la pelota y anotar puntos.",

    image: "/img/pong.png",

    isLocal: false,

    url: "https://ponggame.org/"

  },

  {

    id: 9,

    name: "Space Invaders",

    year: 1978,

    description: "El clásico juego de naves en la pantalla. Un divertido desafío de disparar a alienígenas para hacer puntos.",

    image: "/img/spaceinvaders.png",

    isLocal: false,

    url: "https://freeinvaders.org/"

  },

  {

    id: 10,

    name: "Tetris",

    year: 1984,

    description: "El clásico juego de bloques en la pantalla. Un divertido desafío de encajar piezas para ganar puntos.",

    image: "/img/tetris.png",

    isLocal: false,

    url: "https://tetris.com/play-tetris"

  }

]);

const toggleSortOrder = () => {

  sortOrder.value = sortOrder.value === 'asc' ? 'desc' : 'asc';

};

const displayedGames = computed(() => {

  let result = games.value.filter(g => {

    const term = searchQuery.value.toLowerCase();

    return g.name.toLowerCase().includes(term) || g.description.toLowerCase().includes(term);

  });

  result.sort((a, b) => {

    return sortOrder.value === 'asc' ? a.year - b.year : b.year - a.year;

  });

  return result;

});

// Función para abrir el juego según si es local o externo

const playGame = (game) => {

  if (game.isLocal) {

    activeLocalGame.value = true;

    resetGame();

  }

};

const closeLocalGame = () => {

  activeLocalGame.value = false;

};

// Extrae palabras (incluye acentos) de longitud > 3

const extractWords = (text) => {

  const words = text.toLowerCase().match(/[\p{L}]{4,}/gu);

  return new Set(words || []);

};

// Intersección de palabras clave para todas las descripciones mostradas

const commonKeywords = computed(() => {

  if (displayedGames.value.length === 0) return [];

  let common = extractWords(displayedGames.value[0].description);

  for (let i = 1; i < displayedGames.value.length; i++) {

    const currentWords = extractWords(displayedGames.value[i].description);

    common = new Set([...common].filter(word => currentWords.has(word)));

  }

  return Array.from(common).sort();

});

// --- LÓGICA DEL TRES EN RAYA ---

const board = ref(Array(9).fill(null));

const xIsNext = ref(true);

const winner = computed(() => {

  const lines = [

    [0, 1, 2], [3, 4, 5], [6, 7, 8],

    [0, 3, 6], [1, 4, 7], [2, 5, 8],

    [0, 4, 8], [2, 4, 6]            

  ];

  for (let i = 0; i < lines.length; i++) {

    const [a, b, c] = lines[i];

    if (board.value[a] && board.value[a] === board.value[b] && board.value[a] === board.value[c]) {

      return board.value[a];

    }

  }

  if (board.value.every(cell => cell !== null)) {

    return 'Empate';

  }

  return null;

});

const makeMove = (index) => {

  if (board.value[index] || winner.value) return;

  board.value[index] = xIsNext.value ? 'X' : 'O';

  xIsNext.value = !xIsNext.value;

};

const resetGame = () => {

  board.value = Array(9).fill(null);

  xIsNext.value = true;

};

</script>

<style scoped>

/* Fondo de toda la pantalla */
:global(html),
:global(body),
:global(#app) {
  margin: 0;
  min-height: 100%;
  background-color: #041d2f;
}

/* Contenedor principal */
.app-container {
  font-family: Arial, sans-serif;
  max-width: 1100px;
  margin: 0 auto;
  padding: 20px;
  color: #263047;
  background-color: #041d2f;
}

header {
  text-align: center;
  background: linear-gradient(120deg, #2196f3, #1565c0);
  color: white;
  padding: 30px 15px;
  border-radius: 15px;
  margin-bottom: 30px;
}

header h1 { margin: 0 0 10px; }
header p { margin: 0; }

/* Buscador y botón de ordenar */
.controls { display: flex; gap: 12px; margin-bottom: 25px; }
.search-bar {
  flex: 1;
  padding: 12px;
  border: 1px solid #ccc;
  border-radius: 8px;
  font-size: 15px;
}
.sort-btn, .back-btn {
  background: #6454d8;
  color: white;
  border: none;
  padding: 12px 16px;
  border-radius: 8px;
  cursor: pointer;
}
.sort-btn:hover, .back-btn:hover { background: #4b3bb5; }

/* Tarjetas de juegos */
.games-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(230px, 1fr));
  gap: 20px;
}
.game-card {
  background: white;
  border: 1px solid #e5e5ef;
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 4px 12px rgba(0,0,0,0.08);
  transition: transform 0.2s;
}
.game-card:hover { transform: translateY(-4px); }
.game-img {
  width: 100%;
  height: 160px;
  object-fit: contain;
  background: #f4f2ff;
}
.game-info { padding: 16px; }
.game-info h3 { margin: 0; color: #5141ba; }
.game-info small { color: #777; font-weight: normal; }
.game-info p { color: #555; font-size: 14px; line-height: 1.5; min-height: 85px; }
.play-btn {
  width: 100%;
  padding: 11px;
  background: #6454d8;
  color: white;
  border: none;
  border-radius: 8px;
  font-weight: bold;
  cursor: pointer;
}
.play-btn:hover { background: #4b3bb5; }
.play-btn:disabled { background: #c5c4ce; cursor: not-allowed; }
.no-results { text-align: center; grid-column: 1 / -1; }

/* Tablero del tres en raya */
.minigame-section {
  text-align: center;
  max-width: 480px;
  margin: 0 auto;
  padding: 25px;
  background: #f4f2ff;
  border-radius: 15px;
}
.minigame-section h2 { color: #5141ba; }
.subtitle { color: #666; }
.tictactoe-board {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 8px;
  max-width: 310px;
  margin: 25px auto;
}
.cell {
  aspect-ratio: 1;
  background: white;
  border: 2px solid #d9d3ff;
  border-radius: 10px;
  font-size: 52px;
  font-weight: bold;
  cursor: pointer;
}
.cell:hover { background: #ece8ff; }
.x-cell { color: #ec5e7b; }
.o-cell { color: #6454d8; }
.game-status { font-size: 18px; }
.win-text { color: #218451; font-weight: bold; }
.tie-text { color: #ad7813; font-weight: bold; }
.reset-btn {
  background: #22a073;
  color: white;
  border: none;
  padding: 12px 20px;
  border-radius: 8px;
  cursor: pointer;
}
.reset-btn:hover { background: #19815b; }

/* Pie de página */
footer {
  background: #282a45;
  color: white;
  padding: 20px;
  margin-top: 35px;
  border-radius: 12px;
}
footer h3 { margin-top: 0; }
.keywords-list { display: flex; flex-wrap: wrap; gap: 8px; }
.keyword-tag { background: #6454d8; padding: 7px 12px; border-radius: 15px; }

/* Adaptación para móvil */
@media (max-width: 600px) {
  .controls { flex-direction: column; }
  .minigame-section { padding: 15px; }
}
</style>
