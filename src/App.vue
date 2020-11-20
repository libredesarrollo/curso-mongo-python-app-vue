<template>
  <div class="container">
    <List/>
  </div>
</template>

<script>
import List from './components/ListComponent'
const axios = require("axios");

export default {
  name: 'App',
  created(){
    this.getTags()
    this.getCategories()
  },
  data: function(){
    return {
      tags:[],
      categories:[],
      baseURL: 'http://127.0.0.1:8000/'
    }
  },
  methods: {
    getTags(force = false){
      if(this.tags.length > 0 && !force)
        return

      axios
        .get(this.$root.baseURL + "api/tag/")
        .then((res) => (this.tags = res.data));
    },
    getCategories(force = false){
      if(this.categories.length > 0 && !force)
        return

      axios
        .get(this.$root.baseURL + "api/category/")
        .then((res) => (this.categories = res.data));
    }
  },
  components: {
    List
  }
}
</script>

<style>
@import '../node_modules/bootstrap/dist/css/bootstrap.min.css';
@import '../node_modules/@fortawesome/fontawesome-free/css/all.min.css';
</style>
