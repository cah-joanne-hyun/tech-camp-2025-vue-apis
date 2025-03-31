<script setup lang="ts">
import { ref } from 'vue'
import InputText from 'primevue/inputtext'
import Button from 'primevue/button'

type Movie = {
  Title: string
  Year: string
  imdbId: string
  Poster: string
}

const OMDB_API_KEY = '56648c9'

const query = ref('')
const movies = ref<Movie[]>([])
const error = ref('')

async function searchQuery() {
  movies.value = []
  error.value = ''

  if (!query.value) {
    return
  }

  try {
    const url = `http://www.omdbapi.com/?apikey=${OMDB_API_KEY}&s=${encodeURIComponent(query.value)}&page=1`
    const response = await fetch(url)

    if (!response.ok) {
      throw new Error(`HTTP error! Status: ${response.status}`)
    }

    const data = await response.json()

    if (data.Response === 'True') {
      movies.value = data.Search
      console.log(movies)
    } else {
      error.value = data.Error
    }
  } catch (err) {
    error.value = 'An error occurred while fetching movies'
  }
}
</script>

<template>
  <div>
    <h1>Search for a Movie:</h1>
    <form @submit.prevent="searchQuery">
      <InputText type="text" v-model="query" placeholder="Search for a movie title..." />
      <Button type="submit">Submit</Button>
    </form>

    <div v-if="error" style="color: red">{{ error }}</div>

    <div v-if="movies.length > 0">
      <h2>Results:</h2>
      <div v-for="movie in movies" :key="movie.imdbId">
        <p>{{ movie.Title }}</p>
        <p>{{ movie.Year }}</p>
        <img v-if="movie.Poster !== 'N/A'" :src="movie.Poster" alt="Poster" />
      </div>
    </div>
  </div>
</template>

<style></style>
