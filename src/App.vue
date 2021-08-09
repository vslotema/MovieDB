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
          <v-col :cols="numCols" v-for="i in items" :key="i.imdbID">
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
    key: "20b15791",
    selected: "",
    search: "",
    loading: false,
  }),

  watch: {
    search(val) {
      // console.log("Search ", val);
      if (val && val.length >= 3) this.fetchData();
    },
  },

  computed:{
    numCols(){
      const screen = window.screen.width
      if(screen <= 414) return 12 
      else return 6

    }
  },

  methods: {
    fetchData() {
      this.loading = true;
      axios
        .get(`http://www.omdbapi.com/?apikey=${this.key}&`, {
          params: {
            s: this.search,
            page: 1,
            plot: "full",
          },
        })
        .then((response) => {
          console.log("RESPONSE ", response);
          this.items = response.data.Search;
          //console.log("items ", this.items);
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

.searchbar{
  max-width:750px; 
}

.cards-container{
  max-width: 1024px;
}
</style>
