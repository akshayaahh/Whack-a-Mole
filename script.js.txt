const holes = document.querySelectorAll('.hole');
const scoreEl = document.getElementById('score');
const timeEl = document.getElementById('time');
const startBtn = document.getElementById('startBtn');

let score = 0;
let timeLeft = 30;
let gameInterval;
let moleTimeout;
let gameInProgress = false;

function randomHole() {
  const index = Math.floor(Math.random() * holes.length);
  return holes[index];
}

function showMole() {
  const hole = randomHole();
  hole.classList.add('up');

  // Mole stays up for 700ms
  moleTimeout = setTimeout(() => {
    hole.classList.remove('up');
    if (gameInProgress) showMole();
  }, 700);
}

function startGame() {
  if (gameInProgress) return;
  gameInProgress = true;
  score = 0;
  timeLeft = 30;
  scoreEl.textContent = score;
  timeEl.textContent = timeLeft;

  showMole();

  gameInterval = setInterval(() => {
    timeLeft--;
    timeEl.textContent = timeLeft;
    if (timeLeft <= 0) {
      endGame();
    }
  }, 1000);
}

function endGame() {
  gameInProgress = false;
  clearInterval(gameInterval);
  clearTimeout(moleTimeout);
  holes.forEach(hole => hole.classList.remove('up'));
  alert(`⏰ Time's up! Your score: ${score}`);
}

holes.forEach(hole => {
  hole.addEventListener('click', () => {
    if (hole.classList.contains('up')) {
      score++;
      scoreEl.textContent = score;
      hole.classList.remove('up');
    }
  });
});

startBtn.addEventListener('click', startGame);
