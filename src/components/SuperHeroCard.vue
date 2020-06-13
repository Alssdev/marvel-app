<template>
  <div>
    <div class="card" style="width: 14rem;">
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
.card-img-top {
  height: 220px;
  overflow: hidden;
}
</style>
