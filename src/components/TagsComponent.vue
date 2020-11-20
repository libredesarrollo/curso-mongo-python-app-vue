<template>
  <div class="row">
    <label for>Tag</label>
    <div class="col-md-2">
      <select class="form-control form-control-sm" v-model="tag">
        <option v-for="t in $root.tags" :value="t._id" v-bind:key="t._id">{{ t.name }}</option>
      </select>
    </div>
    <div class="col-md-1">
      <button @click="insert" class="btn btn-sm btn-primary">
        <i class="fa fa-save"></i>
      </button>
    </div>
    <div class="col-md-9">
      <Tag
        @delete-tag="deleteTag"
        v-for="t in mutableBookTags"
        :tag="t"
        :bid="bid"
        v-bind:key="t._id"
      />
    </div>
  </div>
</template>

<script>
const axios = require("axios");
import Tag from "./TagComponent";

export default {
  props: ["bid", "bookTags"],
  data() {
    return {
      tag: "",
      mutableBookTags: this.bookTags,
    };
  },
  created() {
  },
  methods: {
    deleteTag(id) {
      console.log( this.mutableBookTags);

      this.mutableBookTags = this.mutableBookTags.filter(tag => tag._id != id);
console.log( this.mutableBookTags);
      /*this.mutableBookTags.filter(function (tag) {
        if (tag._id == id) return false;
        else return true;
      });*/
    },
    insert: function () {
      if (this.tag == "") {
        console.log("Debe de seleccionar un tag");
        return;
      }

      axios
        .post(this.$root.baseURL + "api/book/" + this.bid + "/add_tag/", {
          tag_id: this.tag,
        })
        .then((book) => {
          console.log(book.data);
          this.mutableBookTags = book.data.tags;
          console.log("Etiqueta asignada");
        });
    },
  },
  components: {
    Tag,
  },
};
</script>