<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png"
          transition="scale-transition"
          width="40"
        />

        <v-img
          alt="Vuetify Name"
          class="shrink mt-1 hidden-sm-and-down"
          contain
          min-width="100"
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-name-dark.png"
          width="100"
        />
      </div>

      <v-spacer></v-spacer>

      <v-btn
        href="https://github.com/vuetifyjs/vuetify/releases/latest"
        target="_blank"
        text
      >
        <span class="mr-2">Latest Release</span>
        <v-icon>mdi-open-in-new</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
      <v-form @submit.prevent="searchUniversities">
        <v-container grid-list-xl mt-8>
          <v-layout wrap justify-center>
            <v-flex xs12>
              <v-window v-model="window">
                <v-window-item key="1" eager>
                  <v-flex text-center>
                    <v-text-field
                      v-model="value"
                      label="Agregue el pais a buscar"
                      outlined
                    ></v-text-field>
                    <v-btn color="primary" @click="searchUniversities"
                      >Buscar</v-btn
                    >
                  </v-flex>
                  <v-flex text-center>
                    <v-simple-table>
                      <template>
                        <thead>
                          <tr>
                            <th class="text-center">Nombre</th>
                            <th class="text-center">Pagina web</th>
                            <th class="text-center">Pais</th>
                            <th class="text-center">Provincia del estado</th>
                          </tr>
                        </thead>
                        <tbody>
                          <tr v-for="item in universities" :key="item.name">
                            <td>{{ item.name }}</td>
                            <td>
                              <a target="_blank" v-bind:href="item.web_pages[0]">{{
                                item.domains[0]
                              }}</a>
                            </td>
                            <td>
                              {{ item.country }}
                            </td>
                            <td>
                              {{ item.state-province ? item.state-province: 'Sin informacion' }}
                            </td>
                          </tr>

                        </tbody>
                      </template>
                    </v-simple-table>
                    <p class="text-center mt-10 gray--text" v-if="noFound">
                          No se encontraron resultados</p>
                  </v-flex>
                </v-window-item>
              </v-window>
            </v-flex>
          </v-layout>
        </v-container>
      </v-form>
    </v-main>
  </v-app>
</template>

<script>
import axios from 'axios';

export default {
  name: 'App',

  components: {},

  data: () => ({
    value: '',
    universities: [],
    noFound: false,
  }),
  methods: {
    searchUniversities() {
      axios
        .get(`http://universities.hipolabs.com/search?country=${this.value}`)
        .then((result) => {
          const { data } = result;
          if (data.length > 0) {
            this.universities = data;
            this.noFound = false;
          } else {
            this.universities = [];
            this.noFound = true;
          }
          console.log(this.universities);
        }).catch((err) => {
          if (err.response.status === 404) {
            throw new Error(`${err.config.url} not found`);
          }
          throw err;
        });
    },
  },
};
</script>
