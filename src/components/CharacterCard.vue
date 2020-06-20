<template>
  <div>
    <div class="card">
      <div class="card-image">
        <figure class="image">
          <img :src="imageUrl" :alt="'image of ' + name" />
        </figure>
      </div>
      <div class="card-content">
        <div class="media">
          <div class="media-content">
            <p class="title">{{name}}</p>
          </div>
        </div>
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

<style scoped lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Open+Sans&display=swap");

.card-content {
  padding-top: 14px;
  padding-bottom: 14px;
  height: 99px;
}

.card {
  transition: 0.2s;
}

.card:hover {
  box-shadow: 0px 0px 9px 1px rgba(0, 0, 0, 0.75);
}

.title {
  font-family: "Open Sans", sans-serif;
  font-size: 20px;
}
</style>
