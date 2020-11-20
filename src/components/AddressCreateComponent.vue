<template>
  <div class="col-md-4">
    <label for>País</label>
    <select class="form-control" v-model="address.country">
      <option value="Venezuela">Venezuela</option>
      <option value="España">España</option>
    </select>

    <label>Dirección</label>

    <textarea class="form-control" v-model="address.direction"></textarea>

    <button @click="create" class="btn btn-sm btn-success mt-2">
      <i class="fa fa-save"></i> Crear
    </button>

    <hr class="bg-primary" />
  </div>
</template>

<script>
const axios = require("axios");

export default {
  props: ["index", "id"],
  data() {
    return {
      address: {
        country: "",
        direction: "",
      },
    };
  },
  methods: {
    create: function () {
      axios
        .post(this.$root.baseURL + "api/book/" + this.id + "/add_address/", {
          country: this.address.country,
          direction: this.address.direction
        })
        .then((book) => {
            console.log(book.data.addresses)
            console.log(book.data.addresses[book.data.addresses.length-1])
            this.$emit("create-address",this.index, book.data.addresses[book.data.addresses.length-1])
            console.log("Crear")
        });
    },
  },
};
</script>