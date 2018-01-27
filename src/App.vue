<template>
  <div id="app">
    <form id="form">
      <p>
        <label>Title: <input type="text" name="title" v-model="book.title"></label>
      </p>
      <p>
        <label>Description: <textarea name="description" v-model="book.description"></textarea></label>
      </p>
      <p>
        <label>Author: 
        <select name="author" v-model="book.author">
          <option v-for="author in authors" :value="author._id">
            {{ author.firstname }} {{ author.lastname }}
          </option>
        </select>
        </label>
      </p>
      <button @click.prevent="create">Create</button>
    </form>
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
      authors: [],
      book: {
        title: '',
        description: '',
        author: ''
      }
    }
  },
  created (){
    this._loadBooks();
    this._loadAuthors();
  },
  methods: {
    _loadBooks(){
      axios.get(`${config.baseURL}/books`,{
        withCredentials: true
      })
      .then( response => {
        this.loading = false;
        this.books = response.data.data;
      })
    },
    _loadAuthors(){
      axios.get(`${config.baseURL}/authors`,{
        withCredentials: true
      })
      .then( response => {
        this.authors = response.data.data;
      })
    },
    create(){
      
      let payload = "";
      Object.keys(this.book).forEach(key => {
        payload+=`${key}=${this.book[key]}&`
      });
      
      axios.post(`${config.baseURL}/books`, payload, {
        withCredentials: true,
        headers: {
          'Content-Type': 'application/x-www-form-urlencoded'
        }
      })
      .then( response => {
        this._loadBooks();
      })
    }
  }
}
</script>