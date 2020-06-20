<template>
  <div>
    <div class="field has-addons">
      <div class="control is-expanded">
        <input class="input" type="text" placeholder="Character name" v-model="text" />
      </div>
      <div class="control">
        <button @click="$emit('click', text)" class="button is-red">Search</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data: function() {
    return {
      text: '',
    };
  },
};
</script>

<style scoped>
input {
  transition: 0.2s;
}
</style>
