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
            <div class="row justify-content-center">
              <div class="col-md-6"> 
                <button type="submit" class="btn btn-primary btn-lg btn-block">Submit</button>
              </div>
            </div>
          </form>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'CreatePost'
  }
  </script>

  