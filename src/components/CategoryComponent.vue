<template>
  <div class="row">
    <label for>Category</label>
    <div class="col-md-4">
      <select class="form-control form-control-sm" v-model="mutableCategory">
        <option v-for="c in $root.categories" :value="c._id" v-bind:key="c._id">
          {{ c.name }}
        </option>
      </select>
    </div>
    <div class="col-md-1">
      <button @click="change" class="btn btn-sm btn-primary">
        <i class="fa fa-save"></i>
      </button>
    </div>
  </div>
</template>

<script>
const axios = require("axios");

export default {
  props: ["category", "bid"],
  data() {
    return {
      mutableCategory: this.category ? this.category._id : '',
    };
  },
  methods: {
    change: function () {
      axios
        .post(this.$root.baseURL + "api/book/" + this.bid + "/set_category/", {
          category_id: this.mutableCategory,
        })
        .then(() => {
          console.log("cambiar");
        });
    },
  },
};
</script>