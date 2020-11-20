<template>
  <div class="modal fade" id="customModal">
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title" id="exampleModalLabel">{{uri}}</h5>
          <button
            type="button"
            class="close"
            data-dismiss="modal"
            aria-label="Close"
          >
            <span aria-hidden="true">&times;</span>
          </button>
        </div>
        <div class="modal-body">
          <p v-if="nameError != ''" class="text-danger">{{ nameError }}</p>
          <input type="text" class="form-control" v-model="name" />
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-secondary" data-dismiss="modal">
            Cerrar
          </button>
          <button type="button" class="btn btn-primary" @click="create">
            Guardar
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
const axios = require("axios");

export default {
  data() {
    return {
      uri: "",
      name: "",
      nameError:""
    };
  },
  methods: {
    showModal(uri) {
      console.log(uri);
      this.uri = uri;
    },
    create() {
      axios
        .post(this.$root.baseURL + "api/" + this.uri + "/", {
          name: this.name,
        })
        .then(() => {
          this.name = "";
          console.log("Creado");

          if(this.uri == "tag")
            this.$root.getTags(true)
          else
            this.$root.getCategories(true)
        })
        .catch((error) => {
          if(error.response.status == 400)
            this.nameError = error.response.data.name[0]
          console.log(error.response);
        });
    },
  },
};
</script>