<template>
  <div>
    <b-field>
      <b-input
        placeholder="Character Name"
        type="search"
        icon="magnify"
        expanded
        :loading="loading"
        v-model="text"
        @keyup.native="search($event.key)"
      ></b-input>

      <p class="control">
        <button @click="search('click')" class="button is-red">Search</button>
      </p>
    </b-field>
  </div>
</template>

<script>
export default {
  props: {
    loading: {
      type: Boolean,
      default: false,
    }
  },

  data: function() {
    return {
      text: '',
    };
  },
  
  methods: {
    search: function(command) {
      if (command === 'Enter') {
        this.$emit('click', this.text);

      } else if (command === 'click') {
        this.$emit('click', this.text);

      }
    }
  }
};
</script>