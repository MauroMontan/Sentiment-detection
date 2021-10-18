<template>
  <v-app>
    <v-main>
      <router-view />
    </v-main>
    <dialogForm />
    <v-footer app>
      <span> Mauro Montaño &copy; {{ new Date().getFullYear() }}</span>
      <v-btn
        href="https://github.com/MauroMontan"
        dark
        class="mx-4 white--text"
        icon
      >
        <v-icon color="black"> mdi-github </v-icon>
      </v-btn>
    </v-footer>
  </v-app>
</template>

<script>
import dialogForm from "../src/components/form.vue";
export default {
  name: "App",
  components: {
    dialogForm,
  },
  computed: {
  },
  methods: {
    goTo(path) {
      this.$router.replace(path);
    
    },
  },
  data: () => ({
  }),
};
</script>
