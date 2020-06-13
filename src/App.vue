<template>
  <div id="app">
    <h1>Marvel Gallery</h1>
    <button @click="fetch">Consultar</button>
  </div>
</template>

<script>
import axios from 'axios';
import env from './env';
export default {
  name: 'App',
  methods: {
    fetch: function() {},
  },
};
</script>

<style></style>
