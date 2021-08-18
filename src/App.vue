<template>
  <v-app>
    <v-main>
      <div class="header pt-6">
        <div class="d-flex justify-center align-content-center">
          <div class="logo mr-3">
            <h1 class="text-center">OMDB</h1>
          </div>

          <v-autocomplete
            class="searchbar"
            :items="items"
            :search-input.sync="search"
            :loading="loading"
            v-model="selected"
            placeholder="Search in our database!"
            clearable
            dense
            solo
            cache-items
            item-text="Title"
            item-value="Title"
            hide-no-data
            return-object
          ></v-autocomplete>
        </div>
      </div>

      <v-container class="cards-container">
    
        <v-row dense>
          <v-col
            align-self="stretch"
            :cols="12"
            sm="6"
            v-for="i in items"
            :key="i.imdbID"
            class="d-flex flex-column align-stretch"
          >
            <MovieCard
              :title="i.Title"
              :poster="i.Poster"
              :type="i.Type"
              :year="i.Year"
              :id="i.imdbID"
            />
          </v-col>
        </v-row>
      </v-container>

      <div class="text-center py-6" v-if="pages > 1">
        <v-pagination
          dark
          color="black"
          v-model="page"
          :length="pages"
          :total-visible="5"
        ></v-pagination>
      </div>
    </v-main>
  </v-app>
</template>

<script>
import axios from "axios";
import MovieCard from "./components/MovieCard.vue";
export default {
  name: "App",

  components: {
    MovieCard,
  },

  data: () => ({
    items: [],
    url:'http://www.omdbapi.com/?apikey=20b15791&',
    selected: "",
    search: "",
    loading: false,
    page: 1,
    pages: 0,

  }),

  watch: {
    search(val) {
      if (val && val.length >= 3) {
        this.fetchData(1);
        sessionStorage.setItem("search", this.search);
      }
    },

    page(val) {
      if (val) this.fetchData(val);
    },
  },

  methods: {
    fetchData(pageNum) {
   
      this.loading = true;
      axios
        .get(this.url, {
          params: {
            s: this.search || sessionStorage.getItem("search"),
            page: pageNum,
            plot: "full",
          },
        })
        .then((response) => {
          this.items = response.data.Search;
          this.pages = Math.ceil(Number(response.data.totalResults) / 10);
        })
        .catch((error) => console.log("error", error))
        .finally(() => (this.loading = false));
    },
  },
};
</script>

<style scoped>
.logo {
  padding: 0.4rem 0.4rem 0.4rem 0.4rem;
  background: #ffba08;
  height: fit-content;
  border-radius: 0.2rem;
}
h1 {
  color: rgb(7, 7, 7);
  font-size: 1rem;
}

.header {
  background: rgb(7, 7, 7);
  padding: 0rem 1rem 0rem 1rem;
}

.v-main {
  background: rgb(36, 36, 36);
}

.searchbar {
  max-width: 700px;
}

.cards-container {
  max-width: 1024px;
}
</style>
