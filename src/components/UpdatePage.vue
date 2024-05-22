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

