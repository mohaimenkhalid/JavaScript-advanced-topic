<script setup>
import {onMounted, ref} from "vue";

const users = ref([])
const userLoading = ref(false)
const getAllUsers = async () => {
  userLoading.value = true
  const response = await fetch("https://jsonplaceholder.typicode.com/users")
  users.value = await response.json();
  userLoading.value = false
}


onMounted( () => {
  const userCardContainer = document.querySelector('.user-card-container')
  const userCardObserver = new IntersectionObserver(async (entries) => {
    const cardContainer = entries[0]
    if(!cardContainer.isIntersecting) return;
    await getAllUsers()
    userCardObserver.unobserve(cardContainer.target)
  }, {
     //threshold: 1,
    rootMargin: '-200px'
  })
  userCardObserver.observe(userCardContainer)
})


</script>

<template>
  <div class="user-card-container" style="height: 500px">
    <h3>User List</h3>
    <div
        v-if="userLoading"
        style="min-height: 400px; height: 400px; display: flex; justify-content: center; justify-items: center; align-items: center">
      Loading....
    </div>
    <div
        v-else
        style="display: flex; flex-wrap: wrap; justify-content: center;">
      <div class="user-card"
           v-for="(user, index) in users"
           :key="index"
           style="margin-right: 5px;"
      >
        <h5 style="margin: 6px 0"> {{user.name}}</h5>
        <div>
          {{user.email}}
        </div>
        <div>{{user.phone}}</div>
      </div>
    </div>
  </div>
</template>

<style>
.user-card {
  width: 40%;
  background: #c1c1c1;
  padding: 1.5rem 2rem;
  border-radius: 1rem;
  margin-bottom: 10px;
}
</style>