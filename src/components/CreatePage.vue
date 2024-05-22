<template>
    <div class="container mt-5">
      <div class="card">
        <div class="card-header bg-dark text-white">
          <h3>Create a New Post</h3>
        </div>
        <div class="card-body">
          <form>
            <div class="form-group">
              <label for="title">Title</label>
              <input type="text" class="form-control" id="title" placeholder="Enter post title">
            </div>
            <div class="form-group mt-3">
              <label for="body">Body</label>
              <textarea class="form-control" id="body" rows="5" placeholder="Enter post body"></textarea>
            </div>
            <div class="form-group mt-3">
              <label for="authors">Authors</label>
              <input type="text" class="form-control" id="authors" placeholder="Enter authors' names">
            </div>
            <div class="row">
            <div class="col">
              <button type="submit" class="btn btn-primary btn-lg btn-block">Post</button>
            </div>
            <div class="col">
              <button type="button" class="btn btn-danger btn-lg btn-block">Discard</button>
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

  