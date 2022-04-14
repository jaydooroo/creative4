<template>
  <div class="post-gallery">
    <div class="post" v-for="item in items" :key="item.id">
      <img :src="item.path" />

      <div class="postInfo">
        <h2>{{ item.title }}</h2>
        <p>{{ item.textarea }}</p>
      </div>
      <div class="edit">
        <div class="form">
          <button @click="deleteItem(item)">Delete</button>
          <button @click="editItem1(item)">Edit</button>
        </div>
        <div class="upload" v-if="findItem">
          <input v-model="findItem.title" />
          <br />
          <br />
          <input class="textareaInput" v-model="findItem.textarea" />
          <input type="file" name="photo" @change="fileChanged" />
          <div class="actions" v-if="findItem">
            <button @click="editItem2(findItem)">post</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src

import axios from "axios";
export default {
  name: "Home",
  data() {
    return {
      title: "",
      file: null,
      addItem: null,
      items: [],
      findTitle: "",
      findItem: null,
      textarea: "",
      findTextarea: "",
    };
  },
  computed: {
    suggestions() {
      let items = this.items.filter((item) =>
        item.title.toLowerCase().startsWith(this.findTitle.toLowerCase())
      );
      return items.sort((a, b) => a.title > b.title);
    },
  },
  created() {
    this.getItems();
  },
  methods: {
    async deleteItem(item) {
      try {
        await axios.delete("/api/items/" + item._id);
        this.findItem = null;
        this.getItems();
        return true;
      } catch (error) {
        console.log(error);
      }
    },
    selectItem(item) {
      this.findTitle = "";
      this.findTextarea = "";
      this.findItem = item;
    },
    fileChanged(event) {
      this.file = event.target.files[0];
    },
    editItem1(item) {
      this.findItem = item;
    },

    async editItem2(item) {
      try {
        const formData = new FormData();
        formData.append("photo", this.file, this.file.name);
        let r1 = await axios.post("/api/photos", formData);
        await axios.put("/api/items/" + item._id, {
          title: this.findItem.title,
          path: r1.data.path,
          textarea: this.findItem.textarea,
        });
        this.findItem = null;
        this.file = null;
        this.getItems();
        return true;
      } catch (error) {
        console.log(error);
      }
    },
    async getItems() {
      try {
        let response = await axios.get("/api/items");
        this.items = response.data;
        return true;
      } catch (error) {
        console.log(error);
      }
    },
  },
};
</script>

<style scoped>
.post-gallery {
  display: flex;
  align-items: center;
  flex-direction: column;
  justify-content: start;
  border-color: red;
  border-width: 10px;
  border-style: solid;
  width: 70%;
  margin-top: 10%;
}

.post {
  display: flex;
  width: 50%;
  margin: 40px;
  align-items: center;
  border-color: rgb(25, 0, 255) d;
  border-width: 10px;
  border-style: solid;
  flex-direction: column;
}

.post img {
  width: 70%;
  border-color: rgb(25, 0, 255) d;
  border-width: 10px;
  border-style: solid;
}

.postInfo {
  margin: 10%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  border-color: rgb(62, 54, 134) d;
  border-width: 10px;
  border-style: solid;
  width: 100%;
}

.postInfo h2 {
  font-size: 30px;
}
</style>
