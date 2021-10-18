<template>
  <div class="text-center">
    <v-dialog v-model="dialog" width="500">
      <template v-slot:activator="{ on, attrs }">
        <v-btn
          color="pink"
          x-large
          dark
          v-bind="attrs"
          v-on="on"
          fixed
          bottom
          right
          fab
        >
          <v-icon>mdi-plus</v-icon>
        </v-btn>
      </template>

      <v-card class="pt-5 pb-3" style="border-radius: 16px">
        <v-card-text>
          <v-text-field
            v-model="form.title"
            filled
            dense
            color="green"
            rounded
            label="Titulo"
            required
          ></v-text-field>
          <v-textarea
            filled
            color="green"
            rounded
            v-model="form.body"
            label="Escribe aquí lo que quieras compartir!, recuerda ser breve"
            required
          ></v-textarea>
          <v-combobox
            v-model="model"
            :filter="filter"
            hint="maximo 8 etiquetas"
            :hide-no-data="!search"
            :items="items"
            :search-input.sync="search"
            hide-selected
            label="click aqui para agregar tiquetas :)"
            multiple
            flat
            small-chips
            solo
          >
            <template v-slot:no-data>
              <v-list-item>
                <span class="subheading">Añade: </span>
                <v-chip :color="`${colors[nonce - 1]} lighten-3`" label small>
                  {{ search }}
                </v-chip>
              </v-list-item>
            </template>
            <template v-slot:selection="{ attrs, item, parent, selected }">
              <v-chip
                v-if="item === Object(item)"
                v-bind="attrs"
                :color="`${item.color} lighten-3`"
                :input-value="selected"
                label
                small
              >
                <span class="pr-2">
                  {{ item.text }}
                </span>
                <v-icon small @click="parent.selectItem(item)">
                  $delete
                </v-icon>
              </v-chip>
            </template>
            <template v-slot:item="{ index, item }">
              <v-text-field
                v-if="editing === item"
                v-model="editing.text"
                autofocus
                flat
                background-color="transparent"
                hide-details
                solo
                @keyup.enter="edit(index, item)"
              ></v-text-field>
              <v-chip
                v-else
                :color="`${item.color} lighten-3`"
                dark
                label
                small
              >
                {{ item.text }}
              </v-chip>
              <v-spacer></v-spacer>
              <v-list-item-action @click.stop>
                <v-btn icon @click.stop.prevent="edit(index, item)">
                  <v-icon>{{
                    editing !== item ? "mdi-pencil" : "mdi-check"
                  }}</v-icon>
                </v-btn>
              </v-list-item-action>
            </template>
          </v-combobox>
          <v-alert type="warning" dense outlined>
            Publicaciones con insultos o que aporten lo opuesto al proposito de
            este proyecto seran eliminadas. Sé amable :)
          </v-alert>
        </v-card-text>

        <v-spacer></v-spacer>

        <v-btn
          style="margin-left: 40%"
          outlined
          rounded
          :loading="loading"
          color="green"
          text
          @click="test"
          >Publicar
        </v-btn>
      </v-card>
    </v-dialog>
  </div>
</template>


<script>
export default {
  data: () => ({
    form: {
      title: "",
      body: "",
      tags: [],
    },
    dialog: false,
    loading: false,
    activator: null,
    attach: null,
    colors: ["green", "purple", "indigo", "cyan", "teal", "orange"],
    editing: null,
    editingIndex: -1,
    items: [
      { header: "selecciona alguna etiqueta o crea una" },
      {
        text: "Consejo",
        color: "blue",
      },
      {
        text: "Motivación",
        color: "indigo",
      },
      {
        text: "inspiración",
        color: "blue",
      },
    ],
    nonce: 1,
    menu: false,
    model: [],
    x: 0,
    search: null,
    y: 0,
  }),

  watch: {
    model(val, prev) {
      if (val.length === prev.length) return;
      if (val.length > 5) {
        this.$nextTick(() => this.model.pop());
      }
      this.model = val.map((v) => {
        if (typeof v === "string") {
          v = {
            text: v,
            color: this.colors[this.nonce - 1],
          };

          this.items.push(v);

          this.nonce++;
        }

        return v;
      });
    },
  },

  methods: {
    test() {
      this.loading = true;
      this.form.tags = this.model;
      let headersList = {
        "Content-Type": "application/json",
      };

      fetch("https://quotes-api.deta.dev/motivational/share-something", {
        method: "POST",
        body: JSON.stringify(this.form),
        headers: headersList,
      })
        .then((response) => {
          return response.text();
        })
        .then(() => {
          this.loading = false;
          this.dialog = false;
          
        });
    },
    edit(index, item) {
      if (!this.editing) {
        this.editing = item;
        this.editingIndex = index;
      } else {
        this.editing = null;
        this.editingIndex = -1;
      }
    },
    filter(item, queryText, itemText) {
      if (item.header) return false;

      const hasValue = (val) => (val != null ? val : "");

      const text = hasValue(itemText);
      const query = hasValue(queryText);

      return (
        text.toString().toLowerCase().indexOf(query.toString().toLowerCase()) >
        -1
      );
    },
  },
};
</script>




<style>
@import url("https://fonts.googleapis.com/css2?family=Indie+Flower&display=swap");
</style>