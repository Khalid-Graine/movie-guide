<template>
  <div class="bg-[#121212] text-white h-fit pt-2">
    <header class="text-center pt-1">
      <p class="font-extrabold text-2xl">
        <i class="fa-solid fa-clapperboard mr-1 text-[#016936]"></i>
        MSA
      </p>
    </header>

    <div class="countainer my-0 mx-auto p-3 flex flex-col justify-center">
      <div class="search-countainer w-12/12 flex flex-wrap mb-1">
        <input
          autofocus
          class="w-full mb-1 h-10 grow pr-3 py-0 pl-5 outline-none border-none text-black rounded-full focu"
          @input="getSearchResults()"
          type="search"
          v-model="movieName"
        />
        <div
          v-if="notFound"
          class="Message mt-3 mx-auto text-[#FF0000] text-sm"
        >
          <i class="fa-solid fa-triangle-exclamation"></i>
          Sorry, the movie you have entered does not exist
        </div>
      </div>

      <div
        v-if="searchResult && !notFound"
        class="result-countainer w-full flex flex-wrap"
      >
        <div class="poster w-[30%] relative h-fit">
          <img
            class="w-full h-full object-contain border-2 border-[#016936]"
            :src="searchResult.Poster"
            alt=""
          />
          <div
            class="absolute top-0 right-0 flex justify-center items-center font-semibold py-[1px] px-1 rounded-bl-xl text-[#fbff00] bg-[#016936]"
          >
            <i class="fa-solid fa-star mr-1"></i>
            <span class="Ratings">9.0</span>
          </div>
        </div>

        <div
          class="informations ml-8 w-7/12 flex flex-col flex-nowrap justify-center"
        >
          <h2 class="Title text-center text-xl font-mono my-4 bg-[#016936]">
            {{ searchResult.Title }}
          </h2>

          <div>
            <i class="fa-regular fa-flag text-[#016936] mr-2"></i>
            <span class="Country">{{ searchResult.Country }}</span>
          </div>
          <div>
            <i class="fa-regular fa-calendar text-[#016936] mr-2"></i>
            <span class="Released">{{ searchResult.Released }}</span>
          </div>
          <div>
            <i class="fa-regular fa-clock text-[#016936] mr-2"></i>
            <span class="Runtime">{{ searchResult.Runtime }}</span>
          </div>
          <div>
            <i class="fa-solid fa-globe text-[#016936] mr-2"></i>
            <span class="Language">{{ searchResult.Language }}</span>
          </div>
        </div>

        <div class="more-info basis-12/12 mt-4">
          <div class="mb-4">
            <h4 class="text-[#016936] font-bold">Plot :</h4>
            <p class="Plot">{{ searchResult.Plot }}</p>
          </div>
          <div>
            <h4 class="text-[#016936] font-bold">Cast :</h4>
            <p class="Actors">{{ searchResult.Actors }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import axios from "axios";

let movieName = ref("");
let searchResult = ref(null);
let searchError = ref(null);
let notFound = ref(false);

const API_ENDPOINT = "http://www.omdbapi.com/";
const API_KEY = "10912a6f";

const getSearchResults = async () => {
  if (movieName.value !== "") {
    try {
      const result = await axios.get(
        `${API_ENDPOINT}?t=${movieName.value}&apikey=${API_KEY}`
      );
      searchResult.value = result.data;
      notFound.value = false;

      if (searchResult.value.Error) {
        notFound.value = true;
      } else {
        notFound.value = false;
      }

      searchError.value = false;
    } catch (error) {
      searchResult.value = null;
      searchError.value = true;
    }
    return;
  }
  searchResult.value = null;
};
</script>
