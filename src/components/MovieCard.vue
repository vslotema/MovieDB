<template>
  <div>
    <v-card :color="'black'" dark min-height="170px">
      <div class="d-flex flex-no-wrap justify-start">
        <v-avatar v-if="poster !== 'N/A'" class="ml-3 my-3" size="145" tile>
          <v-img :src="poster"></v-img>
        </v-avatar>
        <div class="flex-column justify-space-between">
          <v-card-title v-text="title"></v-card-title>

          <v-card-subtitle v-text="year"></v-card-subtitle>
          <v-chip color="#ffba08" x-small class="mx-4">{{ type }}</v-chip>

          <v-card-actions>
            <v-btn color="white" small text @click="show = !show">
              Explore
            </v-btn>

            <v-spacer></v-spacer>

            <v-btn icon @click="show = !show">
              <v-icon>{{
                show ? "mdi-chevron-up" : "mdi-chevron-down"
              }}</v-icon>
            </v-btn>
          </v-card-actions>
        </div>
      </div>

      <v-expand-transition>
        <div v-show="show">
          <v-divider></v-divider>

          <v-card-text>
            {{ plot }}
          </v-card-text>
        </div>
      </v-expand-transition>
    </v-card>
  </div>
</template>

<script>
import axios from "axios";
export default {
  props: {
    title: {
      type: String,
      required: true,
    },
    id: {
      type: String,
      default: "",
    },
    year: {
      type: String,
      default: "",
    },
    type: {
      type: String,
      default: "",
    },
    poster: {
      type: String,
      default: "",
    },
  },
  data() {
    return {
      show: false,
      plot: "",
      key: "20b15791",
      loading: false,
    };
  },

  mounted() {
    this.fetchPlot();
  },
  methods: {
    fetchPlot() {
      this.loading = true;
      axios
        .get(`http://www.omdbapi.com/?apikey=${this.key}&`, {
          params: {
            i: this.id,
            plot: "full",
          },
        })
        .then((response) => {
          console.log("RESPONSE ", response);
          this.plot = response.data.Plot;
          //console.log("items ", this.items);
        })
        .catch((error) => console.log("error", error))
        .finally(() => (this.loading = false));
    },
  },
};
</script>

<style scoped>
.v-card__actions {
  width: 80px;
}

.v-card__subtitle,
.v-card__text,
.v-card__title {
  padding: 13px;
}
</style>
