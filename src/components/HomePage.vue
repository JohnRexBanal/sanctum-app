<template>
  <nav class="navbar">
    <h2>Forum App</h2>
    <div class="container d-flex justify-content-end">
      <ul class="nav">
        <router-link to="/" @click.prevent="logout" class="clo-6">Logout</router-link>
      </ul>
    </div>
  </nav>

  <div class="row">
    <div class="col-12 mb-2 text-end">
    </div>
    <div class="col-12">
      <div class="card m-2">
        <div class="card-header">
          <div class="d-grid gap-2 d-md-flex justify-content-between">
            <h4>Posts</h4>
            <router-link class="btn btn-primary">Create</router-link>
          </div>
        </div>
        <div class="card-body">
          <div class="table-responsive">
            <table class="table table-striped table-bordered">
              <thead>
                <tr class="table-dark">
                  <th>Title</th>
                  <th>Body</th>
                  <th width="15%">Authors</th>
                  <th width="15%">Actions</th>
                </tr>
              </thead>
              <tbody v-if="posts.length > 0">
                <tr v-for="post in posts" :key="post.id">
                  <td>{{ post.title }}</td>
                  <td>{{ post.body }}</td>
                  <td>{{ post.user ? post.user.name : 'Unknown User' }}</td>
                  <td>
                    <div class="d-grid gap-2 d-md-flex justify-content-center">
                      <router-link class="btn btn-success">Edit</router-link>
                      <button type="button" class="btn btn-danger">Delete</button>
                    </div>
                  </td>
                </tr>
              </tbody>
              <tbody v-else>
                <tr>
                  <td colspan="4" align="center">No Categories Found.</td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
  </div>
  <!-- the router view is where all the routes are rendered -->
  <router-view />
</template>

<script>
import axios from 'axios';

export default {
  name: 'HomePage',
  data() {
    return {
      posts: []
    };
  },
  mounted() {
    this.fetchPosts();
  },
  methods: {
    async fetchPosts() {
      try {
        const response = await fetch(this.$root.$data.apiUrl + '/posts');
        if (!response.ok) {
          throw new Error('Failed to fetch posts');
        }
        const data = await response.json(); 
        this.posts = data; 
      } catch (error) {
        console.error(error);
      }
    },
    async logout() {
      try {
        const response = await axios.post(this.$root.$data.apiUrl + '/logout');
        if (response && response.data && response.data.message) {
          console.log(response.data.message);
          localStorage.removeItem('token');
          this.$router.push('/login');
        } else {
          console.error('Unexpected response structure:', response);
        }
      } catch (error) {
        console.error('Error logging out:', error);
        if (error.response && error.response.data && error.response.data.message) {
          this.errors = error.response.data.message;
        } else {
          this.errors = 'An unexpected error occurred. Please try again.';
        }
      }
    }
  }
}
</script>