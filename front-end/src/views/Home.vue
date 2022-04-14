<template>
  <div class="post-gallery">
    <div class="post" v-for="item in items" :key="item.id">
      <div class="postInside">
      <img :src="item.path" />
      <hr/>

      <div class="postInfo">
        <h2>{{ item.title }}</h2>
        <p>{{ item.textarea }}</p>
      </div>
      </div>
      <div class="edit">

        <div class="form">
          <button @click="editItem1(item)"><img src="/edit.png" /></button>
          <button @click="deleteItem(item)"> <img class="logo" src="/delete.png" /></button>
        </div>

        <div class="upload" v-if="findItem">

          <input v-model="findItem.title" />
          <br />
          <br />
          <input class="textareaInput" v-model="findItem.textarea" />
          <input type="file" name="photo" @change="fileChanged" />

          <div class="actions" v-if="findItem">
            <button @click="editItem2(findItem)"><img src="/submit.png" /></button>
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
        if (this.file != null) {
          const formData = new FormData();
          formData.append("photo", this.file, this.file.name);
          let r1 = await axios.post("/api/photos", formData);
          await axios.put("/api/items/" + item._id, {
            title: this.findItem.title,
            path: r1.data.path,
            textarea: this.findItem.textarea,
          });
        } else {
          await axios.put("/api/items/" + item._id, {
            title: this.findItem.title,
            textarea: this.findItem.textarea,
          });
        }
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
  margin: 10%;
}

input {
  font-size: 20px;
}

hr {
  margin-top: 50px;
  border-top: solid 5px purple;
  width: 100%;
}

.post {
  display: flex;
  align-items: center;
  flex-direction: column;
}

.postInside {
  display: flex;
  width: 700px;
  padding: 30px;
  align-items: center;
  border-color: purple;
  border-width: 5px;
  border-style: solid;
  border-radius: 50px;
  flex-direction: column;
}

.post img {
  width: 70%;
}

.actions img {
  width: 35px;
}
.textareaInput {
  width: 100%;
  height: 100px;
  margin-bottom: 10px;
}


.postInfo {
  margin-left: 10%;
  margin-right: 10%;
  margin-bottom: 10%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  width: 100%;
}

.form img {
  width: 35px;
}

.postInfo h2 {
  width: 100%;
  font-size: 30px;
  font-weight: 900;
}

.postInfo p {
  word-break: normal;
  white-space: normal;
  font-size: 25px;
}

.upload {
  display: flex;
  flex-direction: column;
}
</style>
