<script>
  import { onMount } from 'svelte';

  let movieName = '';
  let movieData = null;
  let movieImages = [];

  const TMDB_API_KEY = import.meta.env.MOVIE_KEY;

  async function fetchMovieData() {
    movieData = null;

    try {
      const response = await fetch(`https://api.themoviedb.org/3/search/movie?api_key=${TMDB_API_KEY}&query=${movieName}`);

      if (!response.ok) {
        throw new Error('Film veya dizi bilgisi alınamadı. Lütfen geçerli bir film veya dizi adı girin.');
      }

      const data = await response.json();
      if (data.results && data.results.length > 0) {
        // movieData boş olarak gelecek...
        movieData = {};

        const { title, release_date, cast, budget } = data.results[0];
        movieData.title = title;
        movieData.release_date = release_date;
        movieData.cast = cast;
        movieData.budget = budget;

        const imageResponse = await fetch(`https://api.unsplash.com/search/photos?query=${movieName}&client_id=${'0wfmPjSRJ9cSL4IpNTaxWal8w9nqo6Wo8CWYbwgzeWE'}&per_page=3`);
        const imageData = await imageResponse.json();
        movieImages = imageData.results.map(result => result.urls.small);
      } else {
        throw new Error('Film veya dizi bulunamadı.');
      }
    } catch (error) {
      console.error(error);
      alert(error.message);
    }
  }

  onMount(() => {
    // Sayfa açıldığında movieName değişkeni boş olacak şekilde ayarlandı...
    });
</script>

<main>
  <h1>Film ve Dizi Bilgi Uygulaması</h1>
  <div class="input-container">
    <input bind:value={movieName} placeholder="Film veya Dizi adı girin" />
    <button on:click={fetchMovieData}>Bilgiyi Getir</button>
  </div>

  {#if movieData}
  <div class="movie-container">
    <h2>{movieData.title} ({movieData.release_date})</h2>
    {#if movieData.cast && movieData.cast.length > 0}
      <p>Oyuncular: {movieData.cast.join(', ')}</p>
    {:else}
      <p>Oyuncular: Bilgi Yok</p>
    {/if}
    <p>Yapım Yılı: {movieData.release_date.substr(0, 4)}</p>
    <p>Bütçe: ${movieData.budget}</p>
  </div>

  {#if movieImages.length > 0}
    <div class="image-container">
      {#each movieImages as imageUrl}
        <img src={imageUrl} alt={movieData.title} />
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
    background-image: linear-gradient(to bottom right, #f8f8f8, #eaeaea);
  }

  h1 {
    color: #007bff;
    margin-bottom: 1rem;
  }

  .input-container {
    margin-top: 1rem;
    background-image: linear-gradient(to bottom right, #ffd166, #f8961e);
    padding: 1rem;
    border-radius: 8px;
  }

  input {
    padding: 0.5rem;
    font-size: 1rem;
    border: none;
    border-radius: 4px;
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

  .movie-container {
    background-image: linear-gradient(to bottom right, #8ac926, #2b580c);
    padding: 1rem;
    border-radius: 8px;
    color: white;
    margin-top: 1rem;
  }

  .movie-container h2 {
    color: white;
    font-size: 1.5rem;
    margin-bottom: 0.5rem;
  }

  .movie-container p {
    color: white;
    margin-bottom: 0.5rem;
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
