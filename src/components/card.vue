<template>
  <div>
    <v-card>
      <v-card-title> </v-card-title>
      <v-card-subtitle> </v-card-subtitle>
      <v-card-text> </v-card-text>
    </v-card>
  </div>
</template>

<script>
export default {
  name: "Card",
  props: {
    title: {
      type: String,
      default: "",
    },
    subtitle: {
      type: String,
      default: "",
    },
    text: {
      type: String,
      default: "",
    },
  },
  data() {
    return {};
  },
};
</script>

<style>
</style>