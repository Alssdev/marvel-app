<template>
  <div>
    <div class="card" @click="$emit('click', characterID)">
      <div class="card-image">
        <figure class="image">
          <img :src="imageUrl" :alt="imageAlt" />
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
  name: 'CharacterCard',

  props: {
    imageUrl: {
      type: String,
      default: '#',
    },
    name: {
      type: String,
      required: true,
    },
    characterID: {
      type: Number,
      required: true,
    },
  },

  computed: {
    imageAlt: function() {
      return 'image of ' + this.name;
    }
  },
};
</script>

<style scoped>
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

.image img {
  height: 257px;
}
</style>
