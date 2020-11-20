<template>
  <div
    class="col-md-3 box-update"
    :class="[deleteBox ? 'box-delete' : '']"
    :style="[deleteBoxDisplay ? 'display:none' : '']"
  >

    <button @click="disableInput" class="float-right btn btn-sm btn-link"  :style="[!inputDisable ? 'display:none' : '']">
      <i class="fa fa-save"></i>
    </button>

    <label for>País</label>
    <select :disabled="inputDisable"  class="form-control" v-model="mutableAddress.country">
      <option value="Venezuela">Venezuela</option>
      <option value="España">España</option>
    </select>

    <label>Dirección</label>

    <textarea :disabled="inputDisable" class="form-control" v-model="mutableAddress.direction"></textarea>

    <button @click="remove" class="btn btn-sm btn-danger mt-2 float-right" :style="[inputDisable ? 'display:none' : '']">
      <i class="fa fa-trash"></i>
    </button>
    <button @click="update" class="btn btn-sm btn-primary mt-2"  :style="[inputDisable ? 'display:none' : '']">
      <i class="fa fa-save"></i>
    </button>

    <hr class="bg-primary" />
  </div>
</template>

<script>
const axios = require("axios");

export default {
  props: ["address", "index", "id"],
  data: function () {
    return {
      mutableAddress: this.address,
      deleteBox: false,
      deleteBoxDisplay: false,
      inputDisable: true,
    };
  },
  methods: {
    disableInput() {
      this.inputDisable = !this.inputDisable
    },
    update: function () {
      axios
        .post(this.$root.baseURL + "api/book/" + this.id + "/change_address/", {
          country: this.address.country,
          direction: this.address.direction,
          index: this.index,
        })
        .then(() => console.log("Actualizado"));
    },
    remove: function () {
      axios
        .post(this.$root.baseURL + "api/book/" + this.id + "/remove_address/", {
          index: this.index,
        })
        .then(() => {
          this.deleteBox = true;
          let s = this;
          setTimeout(function () {
            s.deleteBoxDisplay = true;
          }, 400);

          console.log("Eliminado");
        });
    },
  },
};
</script>

<style lang="css">
.box-update:hover {
  /*background: #FFEEEE;
        border: 1px solid #0d6efd;
        */
  transition: 250ms all;
  box-shadow: inset 0 0 0 1px #0d6efd;
}
.box-delete {
  opacity: 0;
  transition: 300ms all;
  width: 0% !important;
  padding: 0 !important;
}
.box-delete button {
  display: none;
}
</style>