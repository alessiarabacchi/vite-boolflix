<script>
export default {
  data() {
    return {
      searchTerm: "",
      results: [],
    };
  },
  methods: {
    search() {
      const apiKey = "813e460999853cac3a67b86a3f46da3c";
      const movieUrl = `https://api.themoviedb.org/3/search/movie?api_key=${apiKey}&language=en-US&query=${this.searchTerm}`;
      const tvUrl = `https://api.themoviedb.org/3/search/tv?api_key=${apiKey}&language=en-US&query=${this.searchTerm}`;

      fetch(movieUrl)
        .then((response) => response.json())
        .then((movies) => {
          fetch(tvUrl)
            .then((response) => response.json())
            .then((tvShows) => {
              this.results = [...movies.results, ...tvShows.results];
            });
        });
    },
    getImageUrl(path) {
      return `https://image.tmdb.org/t/p/w342/${path}`;
    },
    getRating(voteAverage) {
      return Math.ceil(voteAverage / 2);
    },
  },
};
</script>

<template>
  <div id="app">
    <header>
      <img
        src="./components/Leonardo_Diffusion_Character_portrait_of_a_rabbit_girl_with_le_1 (1) - Copia.jpg"
        alt="Movie Search Logo"
      />
      <div class="search-bar">
        <input
          v-model="searchTerm"
          placeholder="Search for a movie or TV show"
        />
        <button @click="search">Search</button>
      </div>
    </header>

    <div id="results" class="results-container">
      <div v-for="result in results" :key="result.id" class="result-card">
        <img :src="getImageUrl(result.poster_path)" alt="Movie Poster" />
        <div class="info">
          <h3>{{ result.title || result.name }}</h3>
          <p>
            Original Title: {{ result.original_title || result.original_name }}
          </p>
          <p>Language: {{ result.original_language }}</p>
          <p>Rating: {{ getRating(result.vote_average) }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped lang="scss">
body {
  font-family: "Arial", sans-serif;
  margin: 0;
  padding: 0;
}

header {
  background-color: #333;
  color: white;
  padding: 10px;
  display: flex;
  justify-content: space-between;
  align-items: center;

  img {
    max-width: 100px;
  }

  .search-bar {
    display: flex;
    gap: 10px;

    input {
      padding: 5px;
      font-size: 14px;
    }

    button {
      padding: 6px 10px;
      font-size: 14px;
      background-color: #4caf50;
      color: white;
      border: none;
      cursor: pointer;
    }
  }
}

.results-container {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  padding: 20px;

  .result-card {
    border: 1px solid #ddd;
    border-radius: 5px;
    overflow: hidden;
    box-shadow: 0 0 5px rgba(0, 0, 0, 0.2);
    transition: transform 0.2s;

    &:hover {
      transform: scale(1.05);
    }

    img {
      width: 100%;
      height: auto;
    }

    .info {
      padding: 10px;

      h3 {
        margin: 0;
        font-size: 16px;
      }

      p {
        margin: 5px 0;
        font-size: 14px;
      }
    }
  }
}
</style>
