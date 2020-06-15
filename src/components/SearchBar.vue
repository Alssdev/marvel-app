<template>
  <div>
    <div class="input-group mb-3">
      <input
        v-model="text"
        type="text"
        class="form-control"
        placeholder="Superhero name"
        aria-label="Superhero name"
        aria-describedby="basic-addon2"
      />
      <div class="input-group-append">
        <button
          class="btn btn-danger"
          type="button"
          @click="$emit('keyup', text)"
        >
          Search
        </button>
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
  font-family: 'Yanone Kaffeesatz';
  font-size: 20px;
  font-weight: 300;
}

.btn-danger {
  background-color: red;
}
.btn-danger:hover {
  background-color: red;
}
</style>
