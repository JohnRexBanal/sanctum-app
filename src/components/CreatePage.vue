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
                <button type="submit" class="btn btn-primary btn-lg btn-block">Post</button>
                <div class="col-md-6">
                <button type="button" class="btn btn-danger btn-lg btn-block">Discard</button>
              </div>
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
    
<style scoped>
.container {
    max-width: 600px;
}

.card-header.bg-dark {
    background-color: #343a40;
}
.card-body {
    border-bottom-left-radius: 15px;
    border-bottom-right-radius: 15px;
}
  
  .btn-primary {
    background-color: #007bff;
    border-color: #007bff;
}
  
  .btn-primary:hover {
    background-color: #0056b3;
    border-color: #0056b3;
}
  
  .btn-primary:focus, .btn-primary.focus {
    box-shadow: 0 0 0 0.2rem rgba(38, 143, 255, 0.5);
}

  .btn-danger {
    background-color: #dc3545;
    border-color: #dc3545;
}
  
  .btn-danger:hover {
    background-color: #c82333;
    border-color: #bd2130;
}
  
  .btn-danger:focus, .btn-danger.focus {
    box-shadow: 0 0 0 0.2rem rgba(220, 53, 69, 0.5);
}
  
  .btn-lg {
    padding: 10px 20px;
    font-size: 1.25rem;
    line-height: 1.5;
    border-radius: 0.3rem;
}
  
  .form-control {
    border-radius: 0.3rem;
    border: 1px solid #ccc;
}
  
  textarea.form-control {
    resize: vertical;
}
  
</style>

  