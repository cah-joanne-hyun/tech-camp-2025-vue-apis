<script setup lang="ts">
import { ref } from 'vue'
import InputText from 'primevue/inputtext'
import Button from 'primevue/button'
import Rating from 'primevue/rating';
import Card from 'primevue/card';


type Movie = {
  Title: string
  Year: string
  imdbId: string
  Poster: string
  rating: number
}

const OMDB_API_KEY = '56648c9'

const query = ref('')
const movies = ref<Movie[]>([])
const error = ref('')
const rating = ref(0)

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
  <div style="padding: 20px;">
    <h2>Search for a Movie:</h2>
    <form @submit.prevent="searchQuery" class="flex justify-content-center">
      <InputText size="large" type="text" v-model="query" placeholder="Search for a movie title..." style="width: 50%" class="margin-10"/>
      <Button type="submit" class="margin-10">Search</Button>
    </form>

    <div v-if="error" style="color: red">{{ error }}</div>

    <div v-if="movies.length > 0">
      <h2>Results:</h2>
      <div id="cards"  style="display:flex; flex-flow:wrap">
      <div  v-for="movie in movies" :key="movie.imdbId">
        <Card class="max-width-min-content margin-10">
          <template #header>
            <img v-if="movie.Poster !== 'N/A'" :src="movie.Poster" alt="Poster" style="height: 427px; width: 284px"/>
          </template>
          <template #title>
            <p>{{ movie.Title }}</p>
          </template>
          <template #subtitle>
            <p>{{ movie.Year }}</p>
            
          </template>
          <template #content>
          </template>
          <template #footer>
            <Rating v-model="movie.rating"></Rating>

          </template>
        </Card>
      </div>
      </div>
    </div>
  </div>
</template>

<style>
.flex {
  display: flex;
}

.justify-content-center {
  justify-content: center;
}

.margin-10 {
  margin: 10px;
}

.max-width-min-content {
  max-width: min-content;
}

</style>
