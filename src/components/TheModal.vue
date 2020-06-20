<template>
  <div>
    <b-modal :active.sync="active" :width="650" scroll="keep">
      <div class="card">
        <div class="card-image">
          <figure class="image">
            <img :src="getImageUrl()" />
          </figure>
        </div>
        <div class="card-content">
          <div class="media">
            <div class="media-content">
              <p class="title is-4">{{character.name}}</p>
            </div>
          </div>

          <div class="content">{{description}}</div>
        </div>
      </div>
    </b-modal>
  </div>
</template>

<script>
export default {
  data: function() {
    return {

    }
  },

  props: {
    isActive: {
      type: Boolean,
      default: false
    },
    character: {
      type: Object
    },
  },

  computed: {
    active: {
      get: function() {
        return this.isActive;
      },
      set: function() {
        this.$emit('deactivate');
        
      }
    },
    description: function() {
      if (this.character.description !== '') {
        return this.character.description

      } else {
        return 'UPS... there is no description';

      }
    }
  },

  methods: {
    getImageUrl: function() {
      return this.character.thumbnail.path + '.' + this.character.thumbnail.extension

    },
  }
}
</script>