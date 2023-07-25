<script>
  let userChoice = "";
  let computerChoice = "";
  let result = "";

  const choices = ["taş", "kağıt", "makas"];
  const winMatrix = {
    taş: "makas",
    kağıt: "taş",
    makas: "kağıt",
  };

  function playGame(userSelection) {
    const randomIndex = Math.floor(Math.random() * choices.length);
    const computerSelection = choices[randomIndex];

    userChoice = userSelection;
    computerChoice = computerSelection;

    if (userChoice === computerChoice) {
      result = "Berabere!";
    } else if (winMatrix[userChoice] === computerChoice) {
      result = "Tebrikler, Kazandınız!";
    } else {
      result = "Üzgünüm, Kaybettiniz!";
    }
  }

  function newGame() {
    userChoice = "";
    computerChoice = "";
    result = "";
  }
</script>

<style>
  main {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 20px;
    background-color: violet;
  }
  .choices {
    display: flex;
    justify-content: space-between;
    width: 200px;
    margin-top: 20px;
  }
  button {
    padding: 10px 20px;
    font-size: 16px;
    border: none;
    background-color: #8c0f19;
    cursor: pointer;
    color: wheat;
  }
  .result {
    font-size: 24px;
    font-weight: bold;
    margin-top: 20px;
  }
</style>

<main>
  <h1>Taş Kağıt Makas Oyunu</h1>

  {#if !userChoice && !computerChoice}
    <p>Yeni oyun için bir seçim yapın:</p>
  {/if}

  <div class="choices">
    {#each choices as choice}
      <button on:click={() => playGame(choice)}>
        {#if choice === "taş"}
          🪨 Taş
        {:else if choice === "kağıt"}
          📄 Kağıt
        {:else}
          ✂️ Makas
        {/if}
      </button>
    {/each}
  </div>

  {#if userChoice && computerChoice}
    <p>Siz: {userChoice}</p>
    <p>Bilgisayar: {computerChoice}</p>
    <p class="result">{result}</p>
    <button on:click={newGame}>Yeni Oyun</button>
  {/if}
</main>
