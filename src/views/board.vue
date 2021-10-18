
<template>
  <v-container class="mt-6">
    <v-app-bar app flat dense color="white">
      <v-spacer />
      <v-btn @click="goTo('/')" icon>
        <v-icon color="yellow"> mdi-arrow-u-left-top-bold </v-icon>
      </v-btn>
      <v-btn @click="reload" icon>
        <v-icon color="success"> mdi-reload </v-icon>
      </v-btn>

      <pageInfo />
    </v-app-bar>
    <v-row class="mx-auto">
      <h1 v-if="cards.length === 0" class="mx-auto">
        Sé el primero en compartir algo ! 😀
      </h1>

      <v-overlay :value="overlay">
        <v-progress-circular
          color="white"
          indeterminate
          size="74"
        ></v-progress-circular>
      </v-overlay>

      <v-col
        v-for="(card, i) in cards"
        :key="i"
        class="mx-auto"
        md="4"
        cols="12"
      >
        <v-card style="border-radius: 14px" elevation="9">
          <v-card-title>
            <h3
              class="mx-auto text-center"
              style="font-family: 'Indie Flower', cursive"
            >
              {{ card.title }}
            </h3>
          </v-card-title>
          <v-card-text>
            <p
              class="px-3 pt-2"
              style="
                border-radius: 16px;
                background-color: #f5f5f5;
                height: 145px;
                overflow-y: scroll;
              "
            >
              {{ card.body }}
            </p>
            <v-chip-group column>
              <v-chip
                v-for="(tag, i) in card.tags"
                :key="i"
                :color="tag.color"
                >{{ tag.text }}</v-chip
              >
            </v-chip-group>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import pageInfo from "../components/pageInfo.vue";
export default {
  name: "Board",
  components: {
    pageInfo,
  },

  created() {
    this.fetchData();
  },
  methods: {
    reload() {
      this.overlay = true;
     
      this.fetchData();
    },

    goTo(path) {
      this.$router.replace(path);
    },
    fetchData() {
      let headersList = {};

      fetch("https://quotes-api.deta.dev/motivational/get-quotes", {
        method: "GET",
        headers: headersList,
      })
        .then((response) => {
          return response.json();
        })
        .then((data) => {
          console.log(data);
          this.cards = data;
          this.overlay = false;
        });
    },
  },
  data() {
    return {
      overlay: true,
      cards: [],
    };
  },
};
</script>


<style>
@import url("https://fonts.googleapis.com/css2?family=Indie+Flower&display=swap");
</style>