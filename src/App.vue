<template>
  <v-app>
    <v-main>
      <router-view />
    </v-main>
    <dialogForm />
    <v-footer app>
      <v-btn href="https://mauros-portfolio.netlify.app/" text>
        <span> Mauro Montaño &copy; {{ new Date().getFullYear() }}</span>
      </v-btn>
      <v-btn small href="https://www.instagram.com/mauro_montano_/" class="mx-2" icon>
        <v-icon >mdi-instagram</v-icon>
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
  computed: {},
  methods: {
    goTo(path) {
      this.$router.replace(path);
    },
  },
  data: () => ({}),
};
</script>
