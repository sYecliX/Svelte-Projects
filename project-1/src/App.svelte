<script>
  import { spring } from 'svelte/motion';

  let cardInfo = {
    name: '',
    cardNumber: '',
    expirationDate: '',
    cvv: '',
  };

  let isCardHolderNameVisible = false;
  let isExpiryDateVisible = false;

  function handlePay() {
    if (cardInfo.name && cardInfo.cardNumber.length === 19 && cardInfo.expirationDate && cardInfo.cvv.length <= 3) {
      alert("Ödemeniz başarıyla gerçekleşti.");
    } else {
      alert("Lütfen tüm bilgileri eksiksiz ve doğru formatta girin.");
    }
  }

  function handleCardNumberInput(event) {
    const { name, value } = event.target;
    const formattedValue = value.replace(/\D/g, '').slice(0, 16).replace(/(.{4})/g, '$1 ').trim();
    cardInfo = { ...cardInfo, [name]: formattedValue };
  }

  function handleNameInput(event) {
    const { name, value } = event.target;
    cardInfo = { ...cardInfo, [name]: value };
    isCardHolderNameVisible = value.length > 0;
  }

  function handleExpirationDateInput(event) {
    const { name, value } = event.target;
    const formattedValue = value.replace(/\D/g, '').slice(0, 4).replace(/(.{2})/, '$1/').trim();
    cardInfo = { ...cardInfo, [name]: formattedValue };
    isExpiryDateVisible = value.length > 0;
  }

  function handleInputChange(event) {
    const { name, value } = event.target;
    cardInfo = { ...cardInfo, [name]: value };
  }

  function sleep(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
  }
</script>

<main>
  <div class="container">
    <div class="card-container">
      <div class="card">
        <div class="front">
          <h3>{cardInfo.cardNumber}</h3>
          <p class:visible={isCardHolderNameVisible}>Card Holder Name v2</p>
          <p>{cardInfo.name ? cardInfo.name : "****"}</p>
          <p class:visible={isExpiryDateVisible}>Expiry Date</p>
          <p>{cardInfo.expirationDate}</p>
        </div>
        <div class="back">
          <p>{cardInfo.cvv}</p>
        </div>
      </div>
    </div>
    <div class="form-container">
      <form>
        <label>
          Kart Numarası:
          <input
            type="text"
            name="cardNumber"
            on:input={handleCardNumberInput}
            bind:value={cardInfo.cardNumber}
            pattern="\d{4}\s?\d{4}\s?\d{4}\s?\d{4}"
            maxlength="19"
          />
        </label>
        <label>
          Card Holder Name:
          <input type="text" name="name" on:input={handleNameInput} bind:value={cardInfo.name} />
        </label>
        <label>
          Expiry Date:
          <input
            type="text"
            name="expirationDate"
            on:input={handleExpirationDateInput}
            bind:value={cardInfo.expirationDate}
            placeholder="MM/YY"
            pattern="\d{2}/\d{2}"
            maxlength="5"
          />
        </label>
        {#if cardInfo.name && cardInfo.cardNumber && cardInfo.expirationDate}
          <label>
            CVV:
            <input
              type="text"
              name="cvv"
              bind:value={cardInfo.cvv}
              pattern="\d{3}"
              maxlength="3"
            />
          </label>
        {/if}
      </form>
      <button on:click={handlePay}>Hesabı Öde</button>
    </div>
  </div>
</main>

<style>
  main {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100vh;
  }

  .container {
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .card-container {
    margin-right: 2rem;
  }

  .form-container {
    margin-left: 2rem;
  }

  .card {
    width: 300px;
    height: 180px;
    position: relative;
    perspective: 1000px;
  }

  .front, .back {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    border-radius: 10px;
    backface-visibility: hidden;
  }

  .front {
    background: linear-gradient(135deg, #F29B9B, #8BC5F2);
    z-index: 2;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding: 1rem;
    color: white;
    font-size: 18px;
    letter-spacing: 1px;
    text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.3);
  }

  .back {
    background: linear-gradient(45deg, #8BC5F2, #F29B9B);
    transform: rotateY(180deg);
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 24px;
    font-weight: bold;
    color: white;
    text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.3);
  }

  p.visible {
    visibility: visible;
  }

  p {
    visibility: hidden;
  }
</style>
