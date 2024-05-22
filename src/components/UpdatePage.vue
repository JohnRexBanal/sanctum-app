<template>
    <div class="container mt-5">
      <div class="card">
        <div class="card-header bg-dark text-white">
          <h3>Update Post</h3>
        </div>
        <div class="card-body">
          <form @submit.prevent="updatePost">
            <div class="form-group">
              <label for="title">Title</label>
              <input type="text" v-model="title" class="form-control" id="title" @input="clearErrors('title')" placeholder="Enter post title">
              <small class="text-danger" v-if="errors.title">{{ errors.title[0] }}</small>
            </div>
            <div class="form-group mt-3">
              <label for="body">Body</label>
              <textarea v-model="body" class="form-control" id="body" rows="5" @input="clearErrors('body')" placeholder="Enter post body"></textarea>
              <small class="text-danger" v-if="errors.body">{{ errors.body[0] }}</small>
            </div>
            <div class="row mt-4">
              <div class="col">
                <button type="submit" class="btn btn-primary btn-lg btn-block">Update</button>
              </div>
              <div class="col">
                <router-link to="/home" class="btn btn-danger btn-lg btn-block">Cancel</router-link>
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
  name: 'UpdatePost',
  data() {
    return {
      title: '',
      body: '',
      errors: {},
      id: ''
    };
  },
  methods: {
    async fetchPost(id) {
      try {
        const response = await axios.get(`${this.$root.$data.apiUrl}/edit/${id}`, {
          headers: {
            Authorization: `Bearer ${localStorage.getItem('token')}`
          }
        });
        this.id = id;
        this.title = response.data.title;
        this.body = response.data.body;
      } catch (error) {
        console.error('Error fetching post:', error);
      }
    },
    async updatePost() {
        try {
          const response = await axios.put(`${this.$root.$data.apiUrl}/update/${this.id}`, {
            title: this.title,
            body: this.body,
          }, {
            headers: {
              Authorization: `Bearer ${localStorage.getItem('token')}`
            }
          });
          if (response.status === 200) {
            alert('Post updated successfully!');
            this.$router.push('/home');
          }
        } catch (error) {
          this.errors = error.response.data.errors;
        }
      },
      clearErrors(field) {
        this.errors[field] = null;
      }
    },
    mounted() {
      const postId = this.$route.params.id;
      this.fetchPost(postId);
    }
  }
</script>




