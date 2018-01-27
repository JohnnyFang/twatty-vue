<template>
  <div id="app">
    <p v-if="loading">Loading ...</p>
    <ul v-else>
      <li v-for="book in books">
        {{ book.title }}
      </li>
    </ul>
  </div>
</template>

<script>
import axios from "axios";
import config from "./config.json";

export default {
  name: 'app',
  data () {
    return {
      loading: true,
      books: []
    }
  },
  created (){
    axios.get(`${config.baseURL}/books`,{
      withCredentials: true
    })
      .then( response => {
        this.loading = false;
        this.books = response.data.data;
      })
  }
}
</script>