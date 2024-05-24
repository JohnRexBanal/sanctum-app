<template>
    <div class="container mt-5">
      <router-link to="/home" class="btn btn-primary mb-3">Back</router-link>
      <div class="card" v-if="post">
        <div class="card-header bg-dark text-white">
          <h3>{{ post.title }}</h3>
        </div>
        <div class="card-body">
          <h5>Author: {{ post.user.name }}</h5>
          <p>{{ post.body }}</p>
        </div>
      </div>
      <div v-else>
        <p>Loading...</p>
      </div>
    </div>
  </template>

<script>
import axios from 'axios';

export default {
  name: 'ViewPage',
  data() {
    return {
      post: null,
    };
  },
  methods: {
    async fetchPost(id) {
      try {
        const response = await axios.get(`${this.$root.$data.apiUrl}/view/${id}`, {
          headers: {
            Authorization: `Bearer ${localStorage.getItem('token')}`
          }
        });
        this.post = response.data;
      } catch (error) {
        console.error('Error fetching post:', error);
      }
    }
  },
  mounted() {
    const postId = this.$route.params.id;
    this.fetchPost(postId);
  }
}
</script>