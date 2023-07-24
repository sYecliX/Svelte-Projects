<script>
  const colors = [
    "red",
    "blue",
    "green",
    "yellow",
    "orange",
    "purple",
    "pink",
    "brown",
  ];

  let cards = generateCards();
  let message = "";
  let remainingAttempts = 5;

  function getRandomColor() {
    const letters = "0123456789ABCDEF";
    let color = "#";
    for (let i = 0; i < 6; i++) {
      color += letters[Math.floor(Math.random() * 16)];
    }
    return color;
  }

  function generateCards() {
    let cards = [];
    for (let color of colors) {
      cards.push({ color: color, isFlipped: false, isMatched: false });
      cards.push({ color: color, isFlipped: false, isMatched: false });
    }
    return shuffleArray(cards);
  }

  function shuffleArray(array) {
    for (let i = array.length - 1; i > 0; i--) {
      const j = Math.floor(Math.random() * (i + 1));
      [array[i], array[j]] = [array[j], array[i]];
    }
    return array;
  }

  let flippedCardIndexes = [];

  function flipCard(index) {
    if (flippedCardIndexes.length < 2 && !cards[index].isFlipped) {
      cards[index].isFlipped = true;
      flippedCardIndexes.push(index);

      if (flippedCardIndexes.length === 2) {
        checkMatch();
      }
    }
  }

  function checkMatch() {
    const [index1, index2] = flippedCardIndexes;
    if (cards[index1].color === cards[index2].color) {
      cards[index1].isMatched = true;
      cards[index2].isMatched = true;

      if (cards.every((card) => card.isMatched)) {
        message = "Tebrikler, kazandınız!";
      }
    } else {
      setTimeout(() => {
        cards[index1].isFlipped = false;
        cards[index2].isFlipped = false;
        remainingAttempts -= 1;

        if (remainingAttempts === 0) {
          message = "Üzgünüz, haklarınız bitti. Kaybettiniz!";
        }
      }, 1000);
    }
    flippedCardIndexes = [];
  }

  function newGame() {
    cards = generateCards();
    flippedCardIndexes = [];
    message = "";
    remainingAttempts = 5;
  }
</script>

<style>
  body {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
    margin: 0;
    background-color: #f7f7f7;
    font-family: Arial, sans-serif;
  }

  .container {
    text-align: center;
    padding: 20px;
    background-color: #fff;
    border-radius: 8px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    max-width: 600px;
  }

  .cards-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 10px;
    margin: 20px 0;
  }

  .card {
    width: 100%;
    height: 100px;
    border-radius: 8px;
    cursor: pointer;
    perspective: 1000px;
    transform-style: preserve-3d;
  }

  .card.flipped .card-front {
    transform: rotateY(-180deg);
  }

  .card.flipped .card-back {
    transform: rotateY(0);
  }

  .card .card-front,
  .card .card-back {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    border-radius: 8px;
    backface-visibility: hidden;
  }

  .card .card-front {
    background-color: #e3e3e3;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 24px;
    font-weight: bold;
    color: #333;
  }

  .card .card-back {
    transform: rotateY(180deg);
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 0;
  }

  button {
    padding: 10px 20px;
    background-color: #007BFF;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    transition: background-color 0.3s ease;
  }

  button:hover {
    background-color: #0056b3;
  }
</style>

<body>
  <div class="container">
    <h1>Hafıza Eşleştirme Oyunu</h1>
    <div class="cards-grid">
      {#each cards as card, index}
        <div
          class="card {card.isFlipped ? 'flipped' : ''} {card.isMatched ? 'matched' : ''}"
          on:click={() => flipCard(index)}
        >
          <div class="card-front"></div>
          <div class="card-back" style="background-color: {card.color}"></div>
        </div>
      {/each}
    </div>
    {#if message}
      <p>{message}</p>
      {#if remainingAttempts === 0}
        <button on:click={newGame}>Yeni Oyun</button>
      {/if}
    {:else}
      <p>Kalan deneme hakkınız: {remainingAttempts}</p>
    {/if}
  </div>
</body>