<template>
  <div id="home">
    <!-- Navigation -->
    <nav class="navbar navbar-expand-lg navbar-light fixed-top" id="mainNav">
      <div class="container">
        <a class="navbar-brand" href="index.html"></a>
        <button
          class="navbar-toggler navbar-toggler-right"
          type="button"
          data-toggle="collapse"
          data-target="#navbarResponsive"
          aria-controls="navbarResponsive"
          aria-expanded="false"
          aria-label="Toggle navigation"
        >
          Menu
          <i class="fas fa-bars"></i>
        </button>
        <div class="collapse navbar-collapse" id="navbarResponsive">
          <ul class="navbar-nav ml-auto">
            <li class="nav-item">
              <a class="nav-link" href="index.html">Home</a>
            </li>
            <li class="nav-item">
              <a class="nav-link" href="about.html">About</a>
            </li>

            <li class="nav-item">
              <a class="nav-link" href="contact.html">Contact</a>
            </li>
          </ul>
        </div>
      </div>
    </nav>

    <!-- Page Header -->
    <header class="masthead" style="background-image: url('img/einstein.jpg')">
      <div class="overlay"></div>
      <div class="container">
        <div class="row">
          <div class="col-lg-8 col-md-10 mx-auto">
            <div class="site-heading">
              <h1>TI Blog</h1>
              <span class="subheading">by Eric Rodriguez</span>
            </div>
          </div>
        </div>
      </div>
    </header>

    <h1>New</h1>
    <div>
      Title
      <input type="text" v-model="newPostTitle" />
      Body:
      <input type="text" v-model="newPostBody" />
      Image:
      <input type="text" v-model="newPostImage" />
      <button v-on:click="createPost()">Create Post</button>
    </div>

        <h1>All Post</h1>
    
              <h2>{{ post.title }}</h2>
              <img v-bind:src="post.url" v-bind:alt="post.title" />
              <button v-on:click="showPost(post)">Show more</button>
              <div v-if="currentPost === post">
                <p>Body: {{ post.body }}</p>
                <p>Image: {{ post.image }}</p>
              </div>
              Title:
              <input type="text" v-model="post.title" />
              Body:
              <input type="text" v-model="post.body" />
              Image:
              <input type="text" v-model="post.image" />
              <button v-on:click="updatePost(post)">Update Post</button>
              <button v-on:click="destroyPost(post)">Destroy Post</button>
            </a>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style></style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      posts: [],
      currentPost: {},
      newPostTitle: "",
      newPostBody: "",
      newPostImage: ""
    };
  },
  created: function() {
    axios.get("/api/posts").then((response) => {
      this.posts = response.data;
    });
  },
  methods: {
    createPost: function() {
      var params = {
        title: this.newPostTitle,
        body: this.newPostBody,
        image: this.newPostImage
      };
      axios.post("/api/posts", params).then((response) => {
        this.posts.push(response.data);
        this.newPostTitle = "";
        this.newPostBody = "";
        this.newPostImage = "";
      });
    },
    showPost: function(post) {
      if (this.currentPost === post) {
        this.currentPost = {};
      } else {
        this.currentPost = post;
      }
    },
    updatePost: function(post) {
      var params = {
        title: post.title,
        body: post.body,
        image: post.image
      };
      axios.patch("/api/posts/" + post.id, params).then((response) => {
        this.currentPost = {};
      });
    },
    destroyPost: function(post) {
      axios.delete("/api/posts/" + post.id).then((response) => {
        var index = this.posts.indexOf(post);
        this.posts.splice(index, 1);
      });
    }
  }
};
</script>
