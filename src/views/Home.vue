<template>
  <v-container fill-height fluid>
    <h1
      class="mx-auto text-center"
      style="font-size: 55px; font-family: 'Indie Flower', cursive"
    >
      Comparte un consejo, una frase, motiva a alguien !
    </h1>
    <p
      class="mx-8 text-justify"
      style="font-size: 28px; font-family: 'Indie Flower', cursive"
    >
      La pandemia ha afectado de diferentes maneras a las personas, entre esas
      personas los estudiantes, que, por causas económicas, desinterés, y falta
      de hábitos han dejado la escuela, o no tienen la intriga de aprender
      nuevas cosas o habilidades útiles que les sean importantes en su
      desarrollo como persona. Es difícil cuando no hay motivación y no tienes
      un propósito claro, busca ayuda y motivación. Cree en ti 😊.
    </p>
    <v-row dense>
      <v-btn class="mx-auto" dark rounded @click="goTo('/board')">
        exploremos!
      </v-btn>
    </v-row>
    <v-img
      max-width="190"
      style="top: 60px; left:70%"
      src="../assets/text.svg"
    >
    </v-img>
  </v-container>
</template>

<script>
export default {
  name: "Home",
  components: {},
  data() {
    return {};
  },
  methods: {
    goTo(path) {
      this.$router.push(path);
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Indie+Flower&display=swap");
::-webkit-scrollbar {
    display: none;
} 
</style>