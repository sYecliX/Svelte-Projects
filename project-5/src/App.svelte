<script>
  let letters = "";
  let userWord = "";
  let resultMessage = "";

  function generateLetters() {
    const alphabet = "ABCÇDEFGHİJKLMNOÖPRSŞTUÜVYZ";
    letters = Array.from({ length: 10 }, () =>
      alphabet.charAt(Math.floor(Math.random() * alphabet.length))
    ).join("");
    userWord = "";
    resultMessage = "";
  }

  function checkUserWord() {
    if (!userWord) {
      resultMessage = "Kelime giriniz.";
      return;
    }

    const words = dictionary.filter((word) => {
      const wordLetters = word.split("");
      return wordLetters.every((letter) => {
        const countInLetters = letters.split(letter).length - 1;
        const countInWord = word.split(letter).length - 1;
        return countInWord <= countInLetters;
      });
    });

    const isWordValid = words.includes(userWord.toUpperCase());

    if (isWordValid) {
      resultMessage = "Tebrikler! Doğru kelime!";
    } else {
      resultMessage = "Maalesef, bu harflerle bu kelime oluşturulamaz.";
    }
  }

  const dictionary = [
    "ARABA",
    "EV",
    "SİTEM",
    "BİLGİSAYAR",
    "KELİME",
    "OYUN",
    "SVELTE",
    "JAVASCRIPT",
    "GELİŞTİRME",
    "KOMPONENT",
    "CHALLENGE",
    "KODLAMA",
    "EĞLENCE",
    "ÇAN",
    "AT"
  ];
</script>

<style>
  main {
    text-align: center;
    padding: 2rem;
  }

  h1 {
    color: #333;
  }

  .letters {
    margin-bottom: 1rem;
  }

  button {
    background-color: #007bff;
    color: #fff;
    padding: 0.5rem 1rem;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    font-size: 1rem;
    margin-top: 0.5rem;
  }

  button:hover {
    background-color: #0056b3;
  }

  .result {
    font-size: 1.2rem;
  }
</style>

<main>
  <h1>Kelime Oyunu</h1>
  <div class="letters">
    <h2>Harfler: {letters}</h2>
    <button on:click={generateLetters}>Yeni Harfler</button>
  </div>
  <div class="user-input">
    <input
      type="text"
      bind:value={userWord}
      placeholder="Kelimenizi girin"
    />
    <button on:click={checkUserWord}>Kelimeyi Kontrol Et</button>
  </div>
  <div class="result">
    {#if resultMessage}
      <p>{resultMessage}</p>
    {/if}
  </div>
</main>
