
<template>
  <div>
    <v-layout justify-center>
      <v-alert
        style="font-family: 'Fredoka One', cursive; font-size: 16px"
        v-model="message"
        class="mx-3"
        type="info"
        border="left"
        dismissible
      >
        Azure Cognitive Service for language busca patrones en el texto para
        determinar una aproximación al sentimiento, solo lo clasifica en
        positivo, negativo o neutral. Solo nosotros podemos determinar con mayor
        exactitud el sentimiento en un texto, es lo que nos hace humanos. 🙂
      </v-alert>
    </v-layout>

    <v-container fluid fill-height>
      <v-dialog v-model="settingsDialog" max-width="450px">
        <v-card style="border-radius: 18px" flat>
          <v-card-title class="headline">
            <v-layout justify-center>
              <p style="font-family: 'Fredoka One', cursive; font-size: 28px">
                Ajustes
              </p>
            </v-layout>
          </v-card-title>

          <v-card-text>
            <v-select
              item-color="orange"
              color="cyan"
              filled
              rounded
              v-model="currentSentiment"
              label="Filtar por sentimiento"
              :items="sentiments"
            ></v-select>
          </v-card-text>
          <v-card-actions>
            <v-layout justify-end>
              <v-btn
                :loading="loadingBySentiment"
                @click="sentimentS"
                color="green"
                rounded
                outlined
              >
                ok
              </v-btn>
            </v-layout>
          </v-card-actions>
        </v-card>
      </v-dialog>

      <v-app-bar hide-on-scroll app flat dense color="white">
        <v-spacer />
        <v-tooltip bottom>
          <template v-slot:activator="{ on, attrs }">
            <v-btn v-bind="attrs" v-on="on" @click="goTo('/')" icon>
              <v-icon color="yellow"> mdi-arrow-u-left-top-bold </v-icon>
            </v-btn>
          </template>
          <span>Regresa a la pagina principal</span>
        </v-tooltip>

        <v-tooltip bottom>
          <template v-slot:activator="{ on, attrs }">
            <v-btn v-bind="attrs" v-on="on" @click="settingsDialog = true" icon>
              <v-icon color="orange"> mdi-tune </v-icon>
            </v-btn>
          </template>
          <span>Filtrar por sentimientos</span>
        </v-tooltip>

        <v-tooltip bottom>
          <template v-slot:activator="{ on, attrs }">
            <v-btn v-bind="attrs" v-on="on" @click="reload" icon>
              <v-icon color="success"> mdi-reload </v-icon>
            </v-btn>
          </template>
          <span>Recarga para ver nuevas publicaciones</span>
        </v-tooltip>
        <pageInfo />
      </v-app-bar>
      <v-row class="mx-auto">
      

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
          md="5"
          cols="12"
        >
          <v-card style="border-radius: 22px" elevation="11">
            <v-layout justify-end>
              <v-tooltip bottom>
                <template v-slot:activator="{ on, attrs }">
                  <v-btn small class="mr-1 mt-1" v-bind="attrs" v-on="on" icon>
                    <v-icon color="red"> mdi-flag </v-icon>
                  </v-btn>
                </template>
                <span>reportar contenido inapropiado(en desarrollo)</span>
              </v-tooltip>
            </v-layout>

            <v-card-title>
              <h3
                class="mx-auto text-center"
                style="font-family: 'Fredoka One', cursive; font-size: 21px"
              >
                {{ card.title }}
              </h3>
            </v-card-title>
            <v-layout justify-center>
              <h4
                style="font-family: 'Fredoka One', cursive; font-size: 15px"
                class="grey--text"
              >
                Sentimiento: {{ translateSentiment(card.sentiment) }}
                <v-icon>{{ setEmoji(card.sentiment) }}</v-icon>
              </h4>
            </v-layout>
            <v-card-text>
              <p
                class="px-3 pt-2"
                style="
                  border-radius: 16px;
                  background-color: #f5f5f5;
                  height: 147px;
                  overflow-y: scroll;
                  font-family: 'Fredoka One', cursive;
                  font-size: 18px;
                "
              >
                {{ card.body }}
              </p>
              <v-chip-group column>
                <v-chip
                  class="mx-auto"
                  outlined
                  style="
                    border: 2px dashed green;
                    font-family: 'Fredoka One', cursive;
                    font-size: 15px;
                  "
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
  </div>
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
    sentimentS() {
      switch (this.currentSentiment) {
        case "positivo":
          this.fetchBySentiment("positive");

          break;
        case "negativo":
          this.fetchBySentiment("negative");
          break;
        case "neutral":
          this.fetchBySentiment("neutral");
          break;
        default:
          this.fetchBySentiment("all");
          break;
      }
    },
    translateSentiment(sentiment) {
      switch (sentiment) {
        case "positive":
          return "Positivo";
          break;
        case "negative":
          return "Negativo";
          break;
        case "neutral":
          return "Neutral";
          break;
        default:
          return "Neutral";
          break;
      }
    },
    setEmoji(sentiment) {
      switch (sentiment) {
        case "positive":
          return "mdi-emoticon-excited";
          break;
        case "neutral":
          return "mdi-emoticon-neutral";
          break;
        case "negative":
          return "mdi-emoticon-sad";
          break;
        default:
          return "mdi-emoticon-neutral";
          break;
      }
    },

    reload() {
      this.overlay = true;

      this.fetchData();
    },

    goTo(path) {
      this.$router.replace(path);
    },
    fetchBySentiment(sentiment) {
      this.message = true;
      this.loadingBySentiment = true;
      let headersList = {};
      let apiUrl =
        "https://quotes-api.deta.dev/motivational/get-quotes/sentiment/" +
        sentiment;
      fetch(apiUrl, {
        method: "GET",
        headers: headersList,
        mod: "no-cors",
      })
        .then((response) => {
          return response.json();
        })
        .then((data) => {
          this.cards = data;
          this.loadingBySentiment = false;
          this.settingsDialog = false;
        });
    },
    fetchData() {
      let headersList = {};

      fetch(
        "https://quotes-api.deta.dev/motivational/get-quotes/sentiment/all",
        {
          method: "GET",
          mod: "no-cors",
          headers: headersList,
        }
      )
        .then((response) => {
          return response.json();
        })
        .then((data) => {
          this.cards = data;
          this.overlay = false;
        });
    },
  },
  data() {
    return {
      message: false,
      sentiments: ["todos", "neutral", "positivo", "negativo"],
      currentSentiment: "negative",
      loadingBySentiment: false,
      overlay: true,
      sentimentEmojis: ["😐", "😃", "😥"],
      settingsDialog: false,
      cards: [],
    };
  },
};
</script>


<style>
@import url("https://fonts.googleapis.com/css2?family=Fredoka+One&display=swap");
@import url("https://fonts.googleapis.com/css2?family=Indie+Flower&display=swap");
</style>