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
          <h2 class="mx-auto grey-text">Reto Master</h2>
        </v-card-title>

        <v-card-text class="text-justify">
          <v-alert outlined border="left" type="info">
            El propósito del reto está enfocado a contribuir y ayudar a
            solucionar un problema en mi comunidad. La implementación de Azure
            Cognitive Service for language ayuda a clasificar los sentimientos
            de los posts y a determinar si son positivos o negativos.
          </v-alert>
          <v-alert border="top" type="warning" outlined>
            Por privacidad he decido no realizar un registro de usuarios.
            Es totalmente anónimo.
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