<template>
  <div>
    <div class="card">
      <div class="card-img-container">
        <img class="card-img-top" :src="imageUrl" :alt="'Image of ' + name" />
      </div>
      <div class="card-body">
        <h5 class="card-title">{{ name }}</h5>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'SuperHeroCard',
  props: {
    imageUrl: {
      type: String,
      default: '#',
    },
    name: {
      type: String,
      required: true,
    },
    uid: {
      type: Number,
      required: true,
    },
  },
};
</script>
<style scoped>
.card {
  margin-bottom: 30px;
  transition: 0.3s;
}

.card-img-top {
  max-height: 250px;
  overflow: hidden;
}

.card:hover {
  box-shadow: 0px 0px 13px -1px rgba(0, 0, 0, 0.75);
}

.card-body {
  height: 100px;
}

.card-title {
  font-family: 'Yanone Kaffeesatz';
  font-size: 23px;
  font-weight: 400;
}
</style>
