<template>
  <div>
    <ul class="list-group">
      <li class="list-group-item" v-for="(b, keyBook) in books" v-bind:key="b._id">
        <button
          class="btn btn-outline-primary btn-sm"
          data-toggle="collapse"
          :data-target="'#_'+b._id"
        >
          <i class="fa fa-arrow-circle-down"></i>
        </button>
        {{ b.name }}
        <div class="collapse mt-2" :id="'_'+b._id">
          <div class="card">
            <div class="card-header">Direcciones</div>
            <div class="card-body">
              <div class="row">
                <Address
                  v-for="(a, key) in b.addresses"
                  v-bind:key="key"
                  :address="a"
                  :id="b._id"
                  :index="key"
                />
                <AddressCreate @create-address="createAddress" :id="b._id" :index="keyBook" />
              </div>
            </div>
          </div>

          <div class="card mt-2">
            <div class="card-header">Etiquetas</div>
            <div class="card-body">
              <Tags :bid="b._id" :bookTags="b.tags" />
            </div>
          </div>
          
          <div class="card mt-2">
            <div class="card-header">Categoría</div>
            <div class="card-body">
              <Category :bid="b._id" :category="b.category" />
            </div>
          </div>

        </div>
      </li>
    </ul>

    <button @click="showModal('category')" class="btn-create-category btn btn-fab btn-primary">
      <i class="fa fa-list"></i>
    </button>
    <button @click="showModal('tag')" class="btn-create-tag btn-create-tag btn btn-fab btn-warning">
      <i class="fa fa-tag text-white"></i>
    </button>

    <CustomModal ref="childModal"/>

  </div>
</template>

<script>
import Address from "./AddressComponent";
import AddressCreate from "./AddressCreateComponent";
import Tags from "./TagsComponent";
import Category from './CategoryComponent'
import CustomModal from "./ModalComponent"

const bootstrap = require('bootstrap')
const axios = require("axios");

export default {
  methods: {
    createAddress(i, address) {
      this.books[i].addresses.push(address);
    },
    showModal(uri) {
      var customModal = new bootstrap.Modal(
        document.getElementById("customModal"),
        {
          keyboard: false,
        }
      );
      this.$refs.childModal.showModal(uri)
      customModal.show()
    },
    
  },
  data: function () {
    return {
      books: [],
    };
  },
  created() {
    axios
      .get(this.$root.baseURL + "api/book/")
      .then((res) => (this.books = res.data));
  },
  components: {
    Address,
    AddressCreate,
    Tags,
    CustomModal,
    Category
  },
};
</script>
<style lang="css">
.btn.btn-fab {
  width: 40px;
  max-width: 40px;
  height: 40px;
  padding: 0;
  border-radius: 50%;
}
.btn-create-category {
  position: fixed;
  bottom: 0;
  right: 0;
  margin: 15px;
}
.btn-create-tag {
  position: fixed;
  bottom: 50px;
  right: 0;
  margin: 15px;
}
</style>