<script setup>
import {ref} from "vue";
import UserList from "@/components/UserList";

const posts = ref([])
const postLoading = ref(false)
const getAllPhotos = async () => {
  postLoading.value = true
  const response = await fetch("https://jsonplaceholder.typicode.com/posts")
  posts.value = await response.json();
  postLoading.value = false
}
getAllPhotos()


</script>

<template>
  <div class="container">
    <div class="nav">
      <h1>Long Page Loader</h1>
    </div>

    <div>
      <h3>Posts List</h3>
      <div
          v-if="postLoading"
          style="min-height: 400px; height: 400px; display: flex; justify-content: center; justify-items: center; align-items: center">
        Loading....
      </div>
      <div
          v-else
          style="display: flex; flex-wrap: wrap; justify-content: center;">
        <div class="post-card"
             v-for="(post, index) in posts"
             :key="index"
             style="margin-right: 5px;"
        >
          <h5 style="margin: 6px 0"> {{post.title}}</h5>
          <div>
            {{post.body}}
          </div>
        </div>
      </div>
    </div>

    <UserList />
  </div>
</template>

<style>
.container {
  margin: auto;
  width: 800px;
}

.nav {
  background: #3F51B5;
  color: white;
  padding: 3px 1.2rem;
  border-radius: 0.5rem;
  font-size: 12px;
}

.post-card {
  width: 40%;
  background: #c1c1c1;
  padding: 1.5rem 2rem;
  border-radius: 1rem;
  margin-bottom: 10px;
}
</style>