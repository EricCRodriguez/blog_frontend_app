<template>
  <div class="home">
    <h1>New Post</h1>
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
    <div v-for="post in posts">
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
