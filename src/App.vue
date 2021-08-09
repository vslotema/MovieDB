<template>
  <v-app>
    <v-main>
      <div class="header">
        <h1 class="text-center">MovieDB</h1>
        <v-autocomplete
          :items="items"
          :search-input.sync="search"
          :loading="loading"
          v-model="selected"
          placeholder="Search in MovieDB"
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
    </v-main>
  </v-app>
</template>

<script>
import axios from "axios";
export default {
  name: "App",

  components: {},

  data: () => ({
    items: [],
    key: "20b15791",
    selected: "",
    search: "",
    loading: false,
  }),

  watch: {
    selected(val) {
      console.log("selected ", val);
    },
    search(val) {
      console.log("Search ", val);
      if (val && val.length >= 3) this.fetchData();
    },
  },

  methods: {
    fetchData() {
      this.loading = true;
      axios
        .get(`http://www.omdbapi.com/?apikey=${this.key}&`, {
          params: {
            s: this.search,
            type: "movie",
            page: 1,
          },
        })
        .then((response) => {
          console.log("RESPONSE ", response);
          this.items = response.data.Search;
          console.log("items ", this.items);
        })
        .catch((error) => console.log("error", error))
        .finally(() => (this.loading = false));
    },
  },
};
</script>

<style scoped>
h1 {
  color: white;
  padding: 1rem 0rem 0.5rem 0rem;
}
.header {
  background: rgb(22, 22, 22);
  padding: 0rem 1rem 0rem 1rem;
}
</style>