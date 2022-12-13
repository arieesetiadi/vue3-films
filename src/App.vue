<template>
    <!-- Homepage -->
    <div class="container flex items-center">
        <div class="w-full p-5">
            <!-- Title -->
            <h1 class="block text-center text-5xl my-5">OUR FILMS</h1>

            <!-- Search box -->
            <form @submit.prevent="searchMovies" class="flex justify-center p-5">
                <input v-model="keyword" type="text" placeholder="Search film.." class="block input w-full border border-slate-500 sm:w-1/2 lg:w-1/3" />
            </form>
        </div>
    </div>

    <!-- Film section -->
    <div class="container">
        <div class="w-full p-5">
            <!-- Title -->
            <h1 class="block text-2xl p-5 border-l border-blue-300">POPULAR FILMS</h1>

            <!-- Film card -->
            <div v-for="(movie, i) in movies" :key="i" class="inline-block px-3 mt-5 w-full sm:w-1/2 md:w-1/2 lg:w-1/3">
                <div class="card shadow-xl border border-slate-500">
                    <figure>
                        <img :src="imgUrl + movie.poster_path" alt="Shoes" class="w-full" />
                    </figure>
                    <div class="card-body min-h-[250px]">
                        <h2 class="card-title">
                            {{ movie.original_title }}
                        </h2>
                        <p>{{ movie.overview.substring(0, 150) }}...</p>
                        <div class="card-actions justify-center mt-3">
                            <div class="badge badge-outline px-5 py-3">{{ movie.release_date }}</div>
                            <div class="badge badge-outline px-5 py-3">⭐{{ movie.vote_average }}</div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import { onMounted, ref } from "vue";
import axios from "axios";

const API_BASE_URL = "https://api.themoviedb.org/3";
const API_KEY = "2ee07de0ae6a0d5fc229ec09d8d6221f";
const API_IMAGE_URL = "https://image.tmdb.org/t/p/w500";
const API_ACCESS_TOKEN = "eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiIyZWUwN2RlMGFlNmEwZDVmYzIyOWVjMDlkOGQ2MjIxZiIsInN1YiI6IjYzNzc0NTM1MTU2Y2M3MDA3NjMwZmNjYiIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.r5kTs4HMjlSEMq4PADN1-YCBjZW_r6knRgcc_8yePPc";

export default {
    setup() {
        let movies = ref([]);
        let keyword = ref("");

        const getMovies = async () => {
            const response = await axios.get(`${API_BASE_URL}/movie/popular?api_key=${API_KEY}`);
            return response.data.results;
        };

        const searchMovies = async () => {
            const response = await axios.get(`${API_BASE_URL}/search/movie?api_key=${API_KEY}&query=${keyword.value}`);
            movies.value = response.data.results;
            keyword.value = "";
        };

        onMounted(async () => {
            movies.value = await getMovies();
        });

        return { movies, keyword, imgUrl: API_IMAGE_URL, searchMovies };
    },
};
</script>
