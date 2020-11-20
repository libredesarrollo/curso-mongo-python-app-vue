<template>
  <div>
    <button
      @click="remove"
      class="btn btn-sm btn-danger float-left ml-1"
      :style="[deleteButtonDisplay ? 'display:none' : '']"
    >
      {{tag.name}}
      <i class="fa fa-window-close"></i>
    </button>
  </div>
</template>

<script>
const axios = require("axios");

export default {
  props: ["tag", "bid"],
  data() {
    return {
      deleteButtonDisplay: false,
    };
  },
  methods: {
    remove: function () {
      axios
        .post(this.$root.baseURL + "api/book/" + this.bid + "/remove_tag/", {
          tag_id: this.tag._id,
        })
        .then(() => {
          this.deleteButtonDisplay = true;
          console.log("Eliminado");
          this.$emit('delete-tag',this.tag._id)
        });
    },
  },
};
</script>