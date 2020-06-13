<template>
  <div id="app">
    <header>
      <nav class="navbar">
        <span class="navbar-brand mb-0 h1">MARVEL APP</span>
      </nav>
      <div id="searchBar">
        <div class="container ">
          <div class="input-group">
            <input
              type="text"
              class="form-control"
              aria-label="Text input with dropdown button"
            />
            <div class="input-group-append">
              <button
                class="btn btn-outline-secondary dropdown-toggle"
                type="button"
                data-toggle="dropdown"
                aria-haspopup="true"
                aria-expanded="false"
              >
                Dropdown
              </button>
              <div class="dropdown-menu">
                <a class="dropdown-item" href="#">Action</a>
                <a class="dropdown-item" href="#">Another action</a>
                <a class="dropdown-item" href="#">Something else here</a>
                <div role="separator" class="dropdown-divider"></div>
                <a class="dropdown-item" href="#">Separated link</a>
              </div>
            </div>
          </div>
        </div>
      </div>
    </header>

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
  </div>
</template>

<script>
import axios from 'axios';
import env from './env';

import SuperHeroCard from './components/SuperHeroCard';

export default {
  name: 'App',

  created: function() {
    this.fetchCharacters();
  },

  data: function() {
    return {
      superHeros: [],
    };
  },

  methods: {
    fetchCharacters: function() {
      const params = env.params;

      axios
        .get(env.endPoint + 'characters', { params })
        .then((response) => {
          this.superHeros = response.data.data.results;
          console.log(this.superHeros);
        })
        .catch((error) => {
          console.log(error);
        });
    },
    getImageUrl: function(superHero) {
      return superHero.thumbnail.path + '.' + superHero.thumbnail.extension;
    },
  },

  components: {
    'super-hero-card': SuperHeroCard,
  },
};
</script>

<style>
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
}

.navbar span {
  font-size: 30px;
  padding: 0px;
}

#searchBar {
  margin-top: 30px;
}
</style>
