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
      ></b-input>

      <p class="control">
        <button @click="$emit('click', text)" class="button is-red">Search</button>
      </p>
    </b-field>
  </div>
</template>

<script>
export default {
  data: function() {
    return {
      text: '',
    };
  },

  props: {
    loading: {
      type: Boolean,
      default: false,
    }
  }
};
</script>