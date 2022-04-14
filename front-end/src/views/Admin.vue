<template>
  <div class="admin">
    <h1>Post</h1>

    <div class="add">
      <div class="form">
        <input class="titleInput" v-model="title" placeholder="Title" />
        <input
          class="textareaInput"
          v-model="textarea"
          placeholder="Description"
        />
        <p></p>
        <input type="file" name="photo" @change="fileChanged" />
        <button class="upload" @click="upload">Upload</button>
      </div>
      <div class="upload" v-if="addItem">
        <h2>{{ addItem.title }}</h2>
        <img :src="addItem.path" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Admin",
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
    async editItem(item) {
      try {
        await axios.put("/api/items/" + item._id, {
          title: this.findItem.title,
          textarea: this.findItem.textarea,
        });
        this.findItem = null;
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
    async upload() {
      try {
        const formData = new FormData();
        formData.append("photo", this.file, this.file.name);
        let r1 = await axios.post("/api/photos", formData);
        let r2 = await axios.post("/api/items", {
          title: this.title,
          path: r1.data.path,
          textarea: this.textarea,
        });
        this.addItem = r2.data;
      } catch (error) {
        console.log(error);
      }
    },
  },
};
</script>
<style scoped>
.admin {
  display: flex;
  padding: 20px;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin-top: 10%;
  border: solid 5px purple;
  border-radius: 10px;
}

.admin h1 {
  font-size: 50px;
  font-weight: bold;
}

.form input {
  font-size: 30px;
  margin-top: 20px;
  margin-bottom: 50px;
}

.image h2 {
  font-style: italic;
  font-size: 50px;
}

.form button {
  font-size: 30px;
}

.heading {
  display: flex;
  margin-bottom: 20px;
  margin-top: 20px;
}

.heading h2 {
  margin-top: 8px;
  margin-left: 10px;
}

.add,
.edit {
  display: flex;
  flex-direction: column;
}

.circle {
  border-radius: 50%;
  width: 18px;
  height: 18px;
  padding: 8px;
  background: #333;
  color: #fff;
  text-align: center;
}

/* Form */
input,
textarea,
select,
button {
  font-family: "Montserrat", sans-serif;
  font-size: 1em;
}

.form {
  display: flex;
  flex-direction: column;
  
}

/* Uploaded images */
.upload h2 {
  margin: 0px;
}

.upload img {
  max-width: 300px;
}
/* Suggestions */
.suggestions {
  width: 200px;
  border: 1px solid #ccc;
}

.suggestion {
  min-height: 20px;
}

.suggestion:hover {
  background-color: #5bdeff;
  color: #fff;
}

.textareaInput {
  width: 100%;
  height: 100px;
}

.titleInput {
  width: 100%;
  margin-bottom: 10px;
}
.upload {
  margin-top: 10px;
}
</style>