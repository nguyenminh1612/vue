<template>
  <div id="app">
    <div class="container">
      <div class="button-wrapper">
        <button class="btn" @click="searchUnsplash('beach')">beach</button>
        <button class="btn" @click="searchUnsplash('winter')">winter</button>
        <button class="btn" @click="searchUnsplash('hot girl')">girl</button>
      </div>
      <stack :column-min-width="300" :gutter-width="15" :gutter-height="15" monitor-images-loaded>
        <stack-item v-for="(image, i) in images" :key="i" style="transition: transform 500ms">
          <img :src="image.urls.small" :alt="image.alt_description" />
        </stack-item>
      </stack>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { Stack, StackItem } from "vue-stack-grid";

export default {
  name: "app",
  components: {
    Stack,
    StackItem
  },
  data: () => ({
    images: []
  }),
  methods: {
    searchUnsplash(topic) {
      this.images = [];
      axios
        .get(
          `https://api.unsplash.com/search/photos?query=${topic}&per_page=50`,
          {
            headers: {
              Authorization:
                "Client-ID u2vt8AQGfd8YjGcW9b_rpqXv_kuh-lxIkBMZZPKGM40",
                "Accept-Version": "v1"
            }
          }
        )
        .then(response => {
          this.images = response.data.results;
        })
        .catch(() => {
          this.images = [];
        });
    }
  }
};
</script>
<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.container {
  width: 80vw;
  margin: 0 auto;
}
.button-wrapper {
  display: flex;
  justify-content: center;
  margin-bottom: 25px;
}
.btn {
  font-size: 18px;
  background-color: #00fff0;
  color: white;
  padding: 10px 20px;
  border-radius: 15px;
}
.btn:not(:last-child) {
  margin-right: 10px;
}
img {
  width: 100%;
  height: auto;
  border-radius: 12px;
}
</style>
