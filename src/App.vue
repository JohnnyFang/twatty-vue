<template>
  <div id="app">
    <div class="offset-md-4" style="padding:5% 5% 5% 5%">
      <div class="card" style="width: 18rem;">
        <div class="card-body">
          <h5 class="card-title">Add new content! </h5>
          <form id="form">
            <p>
              <label>Content: <textarea class="form-control" name="content"
                v-model="post.content"></textarea></label>
            </p>
            <p>
              <label>Location: <input class="form-control" type="text"
                name="location" v-model="post.location"></label>
            </p>
            <p>
              <label>User:
              <select class="form-control"  name="user" v-model="post.user">
                <option v-for="user in users" :value="user._id">
                  {{ user.firstname }} {{ user.lastname }}
                </option>
              </select>
              </label>
            </p>
            <button class="btn btn-primary" @click.prevent="create">Create</button>
          </form>
        </div>
      </div>
    </div>
    <hr />
    <p v-if="loading">Loading ...</p>
    <ul v-else>
      <li class="media" v-for="post in posts" :key="post._id">
        <div class="media-body">
          {{ post.content }}<small><i>, by <b>
            {{ post.user.firstname }} {{ post.user.lastname }}</b></i> from {{post.location}} @ {{post.createdAt | fromNow}} </small>
        </div>
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
      posts: [],
      users: [],
      post: {
        content: '',
        location: '',
        user: ''
      }
    }
  },
  created (){
    this._loadPosts();
    this._loadUsers();
  },
  filters: {
		format(date) {
			return moment(date).format('D.M.YYYY')
		},
		fromNow(date) {
			return moment(date).fromNow();
		}
	},
  methods: {
    _loadPosts(){
      axios.get(`${config.baseURL}/posts`,{
        params:{limit:20},
        // withCredentials: true
      })
      .then( response => {
        this.loading = false;
        this.posts = response.data.data;
      })
    },
    _loadUsers(){
      axios.get(`${config.baseURL}/users`,{
        // withCredentials: true
      })
      .then( response => {
        this.users = response.data.data;
        console.log(response.data);
      })
    },
    resetForm() {
        console.log('Reseting the form')
        this.post.content = '';
        this.post.location = '';
        this.post.user = '';
      },
    create(){

      let payload = "";
      Object.keys(this.post).forEach(key => {
        payload+=`${key}=${this.post[key]}&`
      });

      axios.post(`${config.baseURL}/posts`, payload, {
        // withCredentials: true,
        headers: {
          'Content-Type': 'application/x-www-form-urlencoded',
        }
      })
      .then( response => {
        this._loadPosts();
        this.resetForm();
      })
    }
  }
}
</script>
