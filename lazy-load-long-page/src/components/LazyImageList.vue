<script setup>
import {nextTick, onMounted, ref} from "vue";

const images = ref([])
const imageLoading = ref(false)
const getAllPhotos = async () => {
  imageLoading.value = true
  const response = await fetch("https://jsonplaceholder.typicode.com/photos")
  images.value = await response.json();
  imageLoading.value = false
}


onMounted( () => {
  nextTick(() => {
    setTimeout(() => {
      var lazyImages = [].slice.call(document.querySelectorAll(".lazy-loaded-image.lazy"));
      let lazyImageObserver = new IntersectionObserver(entries => {
        entries.forEach(function(entry) {
          if (entry.isIntersecting) {
            let lazyImage = entry.target;
            lazyImage.src = lazyImage.dataset.src;
            lazyImage.classList.remove("lazy");
            lazyImageObserver.unobserve(lazyImage);
          }
        });
      }, {threshold: 1});
      lazyImages.forEach((lazyImage) => {
        lazyImageObserver.observe(lazyImage);
      });
    }, 100)
  })
})

getAllPhotos()


</script>

<template>
  <div class="image-card-container" style="height: 500px">
    <h3>Image List</h3>
    <div
        v-if="imageLoading"
        style="min-height: 400px; height: 400px; display: flex; justify-content: center; justify-items: center; align-items: center">
      Loading Image....
    </div>
    <div
        v-else
        style="display: flex; flex-wrap: wrap; justify-content: center;">
      <div class="image-card"
           v-for="(image, index) in images"
           :key="index"
           style="margin-right: 5px;"
      >
        <img src="../assets/loading_image.gif"
             :data-src="image.url"
             class="lazy-loaded-image lazy"
             style="width: 100%">
        <h5 style="margin: 6px 0"> {{image.title}}</h5>
      </div>
    </div>
  </div>
</template>

<style>
.image-card {
  width: 40%;
  background: #c1c1c1;
  padding: 1.5rem 2rem;
  border-radius: 1rem;
  margin-bottom: 10px;
}
</style>