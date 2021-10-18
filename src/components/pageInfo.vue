<template>
  <div class="text-center">
    <v-dialog v-model="dialog" width="500">
      <template v-slot:activator="{ on, attrs }">
        <v-btn v-bind="attrs" v-on="on" icon>
          <v-icon color="blue"> mdi-information </v-icon>
        </v-btn>
      </template>

      <v-card flat style="border-radius: 14px">
        <v-card-title class="text-center">
          <h2 class="mx-auto">Reto Master</h2>
        </v-card-title>

        <v-card-text class="text-justify">
          <v-alert
            flat
            border="left"
            colored-border
            color="deep-purple accent-4"
            elevation="2"
          >
            Esta api pronto estara disponible para su implementación en tus
            proyectos, por lo mientras puedes ayudar a contribuir haciendo una
            publicación o leyendo los demás aportes :).
          </v-alert>
          <v-alert flat border="left" colored-border type="info" elevation="2">
            El propósito del reto está enfocado a contribuir y ayudar a
            solucionar un problema en mi comunidad. La implementación de 
            inteligencia artificial ayudara a filtrar las contribuciones
            por etiquetas, prevenir el uso de insultos y evitar
            malos consejos de publicaciones no deseadas
          </v-alert>
        </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary" text @click="dialog = false">
            Entiendo!
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
export default {
  name: "pageInfo",
  data() {
    return {
      dialog: false,
    };
  },
};
</script>
<style>
</style>