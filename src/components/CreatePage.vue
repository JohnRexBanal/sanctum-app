<template>
    <div class="container mt-5">
      <div class="card">
        <div class="card-header bg-dark text-white">
          <h3>Create a New Post</h3>
        </div>
        <div class="card-body">
          <form @submit.prevent="createPost">
            <div class="form-group">
              <label for="title">Title</label>
              <input type="text" class="form-control" id="title" v-model="title" @input="clearErrors('title')" placeholder="Enter post title">
              <small class="text-danger" v-if="errors.title">{{ errors.title[0] }}</small>
            </div>
            <div class="form-group mt-3">
              <label for="body">Body</label>
              <textarea class="form-control" id="body" v-model="body" rows="5" @input="clearErrors('body')" placeholder="Enter post body"></textarea>
              <small class="text-danger" v-if="errors.body">{{ errors.body[0] }}</small>
            </div>
            <div class="row">
            <div class="col">
              <button type="submit" class="btn btn-primary btn-lg btn-block">Post</button>
            </div>
            <div class="col">
              <router-link to="/home" class="btn btn-danger btn-lg btn-block">Discard</router-link>
            </div>
          </div>
          </form>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  export default {
    name: 'CreatePost',
    data() {
    return {
      title: '',
      body: '',
      errors: {},
      posts: [],
    };
  },
  methods: {
    async createPost() {
      try {
        const response = await axios.post(this.$root.$data.apiUrl + '/create', {
          title: this.title,
          body: this.body,
        }, {
          headers: {
            Authorization: `Bearer ${localStorage.getItem('token')}`
          }
        });
        if (response.status === 201) {
          this.posts.push(response.data);
          alert('Post created successfully!');
          this.$router.push('/home');
        }

      } catch (error) {
        this.errors = error.response.data.errors;
      }
    },
    clearErrors(field) {
      this.errors[field] = null;
    }
  }
}
  </script>

  