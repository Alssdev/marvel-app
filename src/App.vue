<template>
  <div id="app">
    <header>
      <nav class="navbar">
        <span class="navbar-brand mb-0 h1">MARVEL APP</span>
      </nav>
      <div class="container">
        <search-bar @click="searchData($event)"></search-bar>
      </div>
    </header>
    <main>
      <div class="container">
        <div class="columns is-deskto is-mobile is-tablet is-multiline is-centered">
          <div
            class="column is-12-mobile is-3-desktop is is-3-tablet"
            v-for="superHero in superHeros"
            :key="superHero.id"
          >
            <character-card
              :imageUrl="getImageUrl(superHero)"
              :name="superHero.name"
              :uid="superHero.id"
            ></character-card>
          </div>
        </div>
      </div>
    </main>
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
</template>

<script>
import axios from "axios";
import env from "./env";

import CharacterCard from './components/CharacterCard';
import SearchBar from './components/SearchBar';
import TheFooter from './components/TheFooter';

export default {
  name: "App",

  created: function() {
    this.fetchSuperHeros();
  },

  data: function() {
    return {
      superHeros: [],
      search: null,
      page: 1,
      limit: 16,
      total: null,
      superHeroSelected: null
    };
  },

  watch: {
    page: function() {
      this.fetchSuperHeros();
    }
  },

  methods: {
    fetchSuperHeros: function() {
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
          this.superHeros = response.data.data.results;
          this.total = response.data.data.total;
        })
        .catch(error => {
          console.log(error);
        });
    },
    getImageUrl: function(superHero) {
      return (
        superHero.thumbnail.path +
        "/standard_fantastic." +
        superHero.thumbnail.extension
      );
    },
    searchData: function(text) {
      if (text == '') {
        this.search = null;
      } else {
        this.search = text;
      }

      this.page = 1;
      this.fetchSuperHeros();
    },
  },

  components: {
    'character-card': CharacterCard,
    'the-footer': TheFooter,
    'search-bar': SearchBar,
  }
};
</script>

<style lang="scss">
@import "./css/bulma_styles.scss";

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

#searchBar {
  margin-top: 30px;
}

main {
  margin-bottom: 40px;
}

.page-link {
  color: red !important;
}
.page-link:focus {
  box-shadow: 0 0 0 0.2rem rgba(255, 0, 0, 0.5) !important;
}

.footer-bar {
  height: 70px;
  background-color: red;
}
</style>
