<template>
  <div id="app">
    <header>
      <nav class="navbar">
        <span class="navbar-brand mb-0 h1">MARVEL APP</span>
      </nav>
      <div id="searchBar">
        <div class="container ">
          <search-bar @keyup="searchData($event)"></search-bar>
        </div>
      </div>
    </header>
    <main>
      <div class="container">
        <div class="row">
          <div
            v-for="superHero in superHeros"
            :key="superHero.id"
            class=" col-12 col-sm-6 col-md-3"
          >
            <super-hero-card
              :uid="superHero.id"
              :name="superHero.name"
              :imageUrl="getImageUrl(superHero)"
            />
          </div>
        </div>
      </div>
    </main>
    <footer>
      <nav aria-label="Page navigation example">
        <ul class="pagination justify-content-center">
          <li class="page-item">
            <button class="btn page-link" href="#" @click="previusPage">
              Previous
            </button>
          </li>
          <li class="page-item">
            <a class="page-link" href="#">{{ page }}</a>
          </li>
          <li class="page-item">
            <button class=" btn page-link" href="#" @click="nextPage">
              Next
            </button>
          </li>
        </ul>
      </nav>
      <div class="footer-bar"></div>
    </footer>
  </div>
</template>

<script>
import axios from 'axios';
import env from './env';

import SuperHeroCard from './components/SuperHeroCard';
import SearchBar from './components/SearchBar';

export default {
  name: 'App',

  created: function() {
    this.fetchSuperHeros();
  },

  data: function() {
    return {
      superHeros: [],
      search: null,
      page: 1,
      superHerosIndex: 0,
      maxSuperHerosIndex: null,
      superHeroSelected: null,
    };
  },

  methods: {
    fetchSuperHeros: function() {
      const params = {
        nameStartsWith: this.search,
        orderBy: '-modified',
        limit: 12,
        offset: this.superHerosIndex, // pagination
        ts: env.ts,
        apikey: env.apikey,
        hash: env.hash,
      };

      axios
        .get(env.endPoint + 'characters', { params })
        .then((response) => {
          this.superHeros = response.data.data.results;
          this.maxSuperHerosIndex = response.data.data.total;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    getImageUrl: function(superHero) {
      return (
        superHero.thumbnail.path +
        '/standard_fantastic.' +
        superHero.thumbnail.extension
      );
    },
    searchData: function(text) {
      if (text) {
        this.search = text;
      } else {
        this.search = null;
      }

      this.page = 1;
      this.superHerosIndex = 0;
      this.fetchSuperHeros();
    },
    nextPage: function() {
      if (this.superHerosIndex + 12 < this.maxSuperHerosIndex) {
        this.superHerosIndex += 12;
        this.page += 1;

        this.fetchSuperHeros();
      }
    },
    previusPage: function() {
      if (this.superHerosIndex - 12 >= 0) {
        this.superHerosIndex -= 12;
        this.page -= 1;

        this.fetchSuperHeros();
      }
    },
  },

  components: {
    'super-hero-card': SuperHeroCard,
    'search-bar': SearchBar,
  },
};
</script>

<style>
@font-face {
  font-family: Marvel;
  src: url('./fonts/MarvelRegular-Dj83.ttf');
}

@font-face {
  font-family: 'Yanone Kaffeesatz';
  src: url('./fonts/YanoneKaffeesatz.ttf');
}

header {
  margin-bottom: 50px;
}

.navbar {
  background-color: red !important;
  font-family: Marvel;
  color: #fff;
  height: 60px;
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
