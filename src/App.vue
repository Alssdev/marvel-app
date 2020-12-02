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
      <div class="container has-text-centered">
        <template v-if="IsCharacterListEmpty">
          <h1 class="title is-1">No characters found yet.</h1>
          <p class="subtitle is-3">¯\_(ツ)_/¯</p>
        </template>

        <div
          class="columns is-deskto is-mobile is-tablet is-multiline is-centered"
        >
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
    <div class="site-footer">
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
    </div>
    <the-modal
      :isActive="isModalCardActive"
      :character="characterSelected"
      @deactivate="isModalCardActive = false"
    ></the-modal>
  </div>
</template>

<script>
import axios from 'axios';
import env from './env';

import CharacterCard from './components/CharacterCard';
import TheSearchBar from './components/TheSearchBar';
import TheFooter from './components/TheFooter';
import TheModal from './components/TheModal';

export default {
  name: 'App',

  components: {
    'character-card': CharacterCard,
    'the-footer': TheFooter,
    'search-bar': TheSearchBar,
    'the-modal': TheModal,
  },

  data: () => ({
    characters: [],
    search: null,
    page: 1,
    limit: 16,
    total: 5,
    loading: false,
    isModalCardActive: false,
    characterSelected: {
      name: '',
      description: '',
      thumbnail: {
        path: '',
        extension: '',
      },
    },
  }),

  computed: {
    IsCharacterListEmpty() {
      return !this.total;
    },
  },

  watch: {
    page() {
      this.fetchCharacters();
    },
  },

  created() {
    this.fetchCharacters();
  },

  methods: {
    async fetchCharacters() {
      const params = {
        nameStartsWith: this.search,
        orderBy: '-modified',
        limit: this.limit,
        offset: this.page * this.limit - this.limit, // pagination
        ts: env.ts,
        apikey: env.apikey,
        hash: env.hash,
      };

      this.loading = true;
      try {
        const response = await axios.get(env.endPoint + 'characters', {
          params,
        });

        this.characters = response.data.data.results;
        this.total = response.data.data.total;
      } catch (error) {
        this.$buefy.notification.open({
          message: 'There was an error communicating with the API',
          duration: 5000,
          type: 'is-danger',
        });
      }
      this.loading = false;
    },
    async fetchOneCharacter(characterID) {
      const params = {
        ts: env.ts,
        apikey: env.apikey,
        hash: env.hash,
      };

      try {
        const response = await axios.get(
          env.endPoint + 'characters/' + characterID,
          { params }
        );

        this.characterSelected = response.data.data.results[0];
        this.isModalCardActive = true;
      } catch (error) {
        this.$buefy.notification.open({
          message: 'There was an error communicating with the API',
          duration: 5000,
          type: 'is-danger',
        });
      }
    },
    getImageUrl(character) {
      return character.thumbnail.path + '.' + character.thumbnail.extension;
    },
    searchData(text) {
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
};
</script>

<style lang="scss">
@import './css/bulma_styles.scss'; // bulma config

@font-face {
  font-family: Marvel;
  src: url('./fonts/MarvelRegular-Dj83.ttf');
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
