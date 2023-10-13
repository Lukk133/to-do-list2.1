<template>
 
<div class="container">
  
  <nav class="navbar navbar-expand-lg bg-body-tertiary">
    <div class="container-fluid">
      <a class="navbar-brand" href="#">Placeholder</a>
      <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent"
        aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav me-auto mb-2 mb-lg-0">
          <router-link class="nav-link" to="/">Home</router-link>
          <li class="nav-item">
            <router-link class="nav-link" to="/about">About</router-link>
          </li>
        </ul>
        <form class="d-flex" role="search">
        <!--<router-link class="nav-link" to="/login">Login</router-link>-->  
          
          <button class="btn btn-outline-primary" type="submit">Register</button>
        </form>
      </div>
    </div>
  </nav>
  <!--<br>-->
</div>
</template>

<script>

export default {
  name: 'App',
  components: {
    
  }
}

</script>