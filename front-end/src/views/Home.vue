<template>
<div class="home">
  <div class="post-gallery">
    <div class="post" v-for="item in items" :key="item.id">
      <img :src="item.path" />

      <div class="postInfo">
        <h2>{{item.title}}</h2>
        <p>{{item.content}}content</p>
      </div>

    </div>
  </div>
</div>
</template>

<script>
// @ is an alias to /src

import axios from 'axios';
export default {
  name: 'Home',
    data() {
    return {
     items: [],
    }
  },
    created() {
    this.getItems();
  },
    methods: {
    async getItems() {
      try {
        let response = await axios.get("/api/items");
        this.items = response.data;
        return true;
      } catch (error) {
        console.log(error);
      }
    },
  }
}
</script>

<style scoped>
.home {
  display: flex;
  justify-content: center;
}

.post-gallery {
  margin:;
  display: flex;
  flex-direction: column;
  justify-content: start;

}

.post {
  display: flex;
  width: 50%;
  margin: 40px;
}

.post img {
  width: 50%;
}

.postInfo {
  margin: 30px;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.postInfo h2 {
  font-size: 30px;
}

</style>
