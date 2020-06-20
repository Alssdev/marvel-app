<template>
  <div id="app">
    <header>
      <nav class="navbar">
        <span class="navbar-brand mb-0 h1">MARVEL APP</span>
      </nav>
      <div class="container">
        <search-bar @click="searchData($event)" :loading="loading"></search-bar>
      </div>
    </header>
    <main>
      <div class="container">
        <div class="columns is-deskto is-mobile is-tablet is-multiline is-centered">
          <div
            class="column is-12-mobile is-3-desktop is is-3-tablet"
            v-for="character in characters"
            :key="character.id"
          >
            <character-card
              :imageUrl="getImageUrl(character)"
              :name="character.name"
              :characterID="character.id"
              @click="fetchOneCharacter($event)"
            ></character-card>
          </div>
        </div>
      </div>
    </main>
    <footer>
      <div class="container">
        <b-pagination
          type="is-red"
          :total="total"
          :current.sync="page"
          :per-page="16"
          order="is-centered"
        ></b-pagination>
      </div>
      <the-footer></the-footer>
    </footer>
    <the-modal
      :isActive="isModalCardActive"
      :character="characterSelected"
      @deactivate="isModalCardActive=false"
    ></the-modal>
  </div>
</template>

<script>
import axios from "axios";
import env from "./env";

import CharacterCard from './components/CharacterCard';
import TheSearchBar from './components/TheSearchBar';
import TheFooter from './components/TheFooter';
import TheModal from './components/TheModal';

export default {
  name: "App",

  created: function() {
    this.fetchCharacters();
  },

  data: function() {
    return {
      characters: [],
      search: null,
      page: 1,
      limit: 16,
      total: null,
      loading: false,
      isModalCardActive: false,
      characterSelected: {
        name: '',
        description: '',
        thumbnail: {
          path: '',
          extension: '',
        }
      },
    };
  },

  watch: {
    page: function() {
      this.fetchCharacters();
    }
  },

  methods: {
    fetchCharacters: function() {
      const params = {
        nameStartsWith: this.search,
        orderBy: "-modified",
        limit: this.limit,
        offset: (this.page * this.limit) - this.limit, // pagination
        ts: env.ts,
        apikey: env.apikey,
        hash: env.hash
      };

      axios
        .get(env.endPoint + "characters", { params })
        .then(response => {
          this.characters = response.data.data.results;
          this.total = response.data.data.total;

          this.loading = false;
        })
        .catch(error => {
          console.log(error);

          this.loading = false;
        });
    },
    fetchOneCharacter: function(characterID) {
      const params = {
        ts: env.ts,
        apikey: env.apikey,
        hash: env.hash
      };

      axios
      .get(env.endPoint + "characters/" + characterID, { params })
        .then(response => {
          this.characterSelected = response.data.data.results[0];
          
          this.isModalCardActive = true;
        })
        .catch(error => {
          console.log(error);
        });
    },
    getImageUrl: function(character) {
      return character.thumbnail.path + '.' + character.thumbnail.extension

    },
    searchData: function(text) {
      this.loading = true;

      if (text == '') {
        this.search = null;

      } else {
        this.search = text;

      }

      this.page = 1;
      this.fetchCharacters();
    },
  },

  components: {
    'character-card': CharacterCard,
    'the-footer': TheFooter,
    'search-bar': TheSearchBar,
    'the-modal': TheModal,
  }
};
</script>

<style lang="scss">
@import "./css/bulma_styles.scss"; // bulma config

@font-face {
  font-family: Marvel;
  src: url("./fonts/MarvelRegular-Dj83.ttf");
}

header {
  margin-bottom: 50px;
}

.navbar {
  background-color: red !important;
  font-family: Marvel;
  color: #fff;
  height: 60px;
  margin-bottom: 30px;
}

.navbar span {
  font-size: 30px;
  padding: 0px;
}

main {
  margin-bottom: 40px;
}
</style>
