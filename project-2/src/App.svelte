<script>
  import { onMount } from 'svelte';

  let cityName = '';
  let weatherData = null;
  let cityImages = [];
  
  const API_KEY = import.meta.env.WEATHER_KEY;
  const UNSPLASH_API_KEY = import.meta.env.UNSPLASH_KEY;

  async function fetchWeatherData() {
    weatherData = null;

    try {
      const response = await fetch(`https://api.openweathermap.org/data/2.5/weather?q=${cityName}&appid=${API_KEY}`);

      if (!response.ok) {
        throw new Error('Hava durumu verisi alınamadı. Lütfen geçerli bir şehir ya da ülke girin.');
      }

      const data = await response.json();
      weatherData = data;

      const imageResponse = await fetch(`https://api.unsplash.com/search/photos?query=${cityName}&client_id=${UNSPLASH_API_KEY}&per_page=3`);
      const imageData = await imageResponse.json();
      cityImages = imageData.results.map(result => result.urls.small);
    } catch (error) {
      console.error(error);
      alert(error.message);
    }
  }

  onMount(() => {
    // Sayfa açılırken cityName boş olarak ayarlandı... 
  });
</script>

<main>
  <h1>Hava Durumu Uygulaması</h1>
  <div class="input-container">
    <input bind:value={cityName} placeholder="Şehir ya da Ülke girin" />
    <button on:click={fetchWeatherData}>Hava Durumunu Getir</button>
  </div>

  {#if weatherData}
    <div class="weather-container">
      <h2>{weatherData.name} için Hava Durumu</h2>
      <p>Sıcaklık: {(weatherData.main.temp - 273.15).toFixed(2)} °C</p>
      <p>Hissedilen Sıcaklık: {(weatherData.main.feels_like - 273.15).toFixed(2)} °C</p>
      <p>Rüzgar Hızı: {weatherData.wind.speed} m/s</p>
    </div>

    {#if cityImages.length > 0}
      <div class="image-container">
        {#each cityImages as imageUrl}
          <img src={imageUrl} alt={weatherData.name} />
        {/each}
      </div>
    {/if}
  {/if}
</main>

<style>
  main {
    text-align: center;
    padding: 1rem;
    background-color: #f8f8f8;
  }

  h1 {
    color: #007bff;
    margin-bottom: 1rem;
  }

  .input-container {
    margin-top: 1rem;
  }

  input {
    padding: 0.5rem;
    font-size: 1rem;
  }

  button {
    padding: 0.5rem 1rem;
    background-color: #007bff;
    color: white;
    border: none;
    border-radius: 4px;
    margin-left: 0.5rem;
    cursor: pointer;
  }

  .weather-container {
    border: 1px solid #ccc;
    border-radius: 4px;
    padding: 1rem;
    margin-top: 1rem;
  }

  .image-container {
    display: flex;
    justify-content: space-around;
    margin-top: 1rem;
  }

  img {
    width: 300px;
    height: 200px;
    object-fit: cover;
    border-radius: 4px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  }

</style>
