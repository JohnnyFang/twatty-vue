<template>
  <div id="app">
    <div>
      <p>
        <label>Title: <input type="text" v-model="book.title"></label>
      </p>
      <p>
        <label>Description: <textarea v-model="book.description"></textarea></label>
      </p>
      <p>
        <label>Author: <input type="text" v-model="book.author"></label>
      </p>
      <button @click="create">Create</button>
    </div>
    <hr />
    <p v-if="loading">Loading ...</p>
    <ul v-else>
      <li v-for="book in books" :key="book._id">
        {{ book.title }} - {{ book.author.firstname }} {{ book.author.lastname }}
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
      books: [],
      book: {
        title: '',
        description: '',
        author: ''
      }
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
  },
  methods: {
    create(){
      axios({
        method: 'post',
        url: `${config.baseURL}/books`,
        data: this.book,
        withCredentials: true
      })
      .then( response => {
        this.books.shift(response.data.data);
      })
    }
  }
}
</script>