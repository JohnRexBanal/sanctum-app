<template>
  <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
    <div class="container">
      <h2 class="navbar-brand">Forum App</h2>
      <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse justify-content-end" id="navbarNav">
        <ul class="navbar-nav">
          <li class="nav-item">
            <span v-if="currentUser" class="nav-link">Logged in as: {{ currentUser.name }}</span>
          </li>
          <li class="nav-item">
            <router-link to="/" @click.prevent="logout" class="nav-link">Logout</router-link>
          </li>
        </ul>
      </div>
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
            <router-link to="/create" class="btn btn-primary">Create</router-link>
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
                      <router-link :to="'/view/' + post.id" class="btn btn-primary">View</router-link>
                      <template v-if="isOwner(post)">
                        <router-link :to="'/update/' + post.id" class="btn btn-success">Edit</router-link>
                        <button @click="deletePost(post.id)" class="btn btn-danger">Delete</button>
                      </template>

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
      posts: [],
      userId: null,
      currentUser: null
    };
  },
  async mounted() {
    await this.fetchPosts();
    this.userId = await this.fetchUserId();
    this.currentUser = await this.fetchCurrentUser();
  },
  methods: {
    async fetchPosts() {
      try {
        const token = localStorage.getItem('token');
        const response = await axios.get(this.$root.$data.apiUrl + '/posts', {
          headers: {
            Authorization: `Bearer ${token}`
          }
        });

        if (response.status === 200) {
          this.posts = response.data;
        } else {
          throw new Error('Failed to fetch posts');
        }
      } catch (error) {
        console.error('Error fetching posts:', error);
      }
    },
    async fetchUserId() {
      try {
        const response = await axios.get(`${this.$root.$data.apiUrl}/user`, {
          headers: {
            Authorization: `Bearer ${localStorage.getItem('token')}`
          }
        });
        return response.data.id;
      } catch (error) {
        console.error('Error fetching user ID:', error);
      }
    },
    async fetchCurrentUser() {
      try {
        const token = localStorage.getItem('token');
        const response = await axios.get(`${this.$root.$data.apiUrl}/user`, {
          headers: {
            Authorization: `Bearer ${token}`
          }
        });
        return response.data;
      } catch (error) {
        console.error('Error fetching current user:', error);
        return null;
      }
    },
    isOwner(post) {
      return post.user_id === this.userId;
    },
    async logout() {
      try {
        const token = localStorage.getItem('token');
        const response = await axios.post(this.$root.$data.apiUrl + '/logout', {}, {
          headers: {
            'Authorization': `Bearer ${token}`
          }
        });
        if (response && response.data && response.data.message) {
          console.log(response.data.message);
          localStorage.removeItem('token');
          this.$router.push('/');
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
    },
    async deletePost(id) {
      if (confirm("Are you sure you want to delete this post?")) {
        try {
          await axios.delete(`${this.$root.$data.apiUrl}/delete/${id}`, {
            headers: {
              Authorization: `Bearer ${localStorage.getItem('token')}`
            }
          });
          this.posts = this.posts.filter(post => post.id !== id);
        } catch (error) {
          console.error('Error deleting post:', error);
          if (error.response && error.response.status === 403) {
            // Forbidden: User doesn't have permission to delete the post
            alert('Unauthorized: You do not have permission to delete this post.');
          } else {
            alert('An error occurred while deleting the post.');
          }
        }
      }
    }
  }
};
</script>