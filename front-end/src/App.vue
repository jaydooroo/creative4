<template>
  <div id="app">
    <div class="header">
      <div class="title">
        <h1>Fakestagram</h1>
      </div>

      <div class="menuButton">
        <router-link to="/">Home</router-link>
      </div>

      <div class="menuButton">
        <router-link to="/admin">New Post</router-link>
      </div>
    </div>
    <div class="content">
      <router-view />
    </div>
    <div class="footer">
      <div>
        <a href="https://github.com/BYUCS260/museum-JJayLeee.git"
          >https://github.com/BYUCS260/museum-JJayLeee.git</a>

          <p>Jaehyun Lee AND Jonghyun Lee</p>
      </div>
    </div>
  </div>
</template>

<style>
html {
  box-sizing: border-box;
}

body {
  font-family: "Montserrat", sans-serif;
  font-size: 16px;
  background: #fff;
  padding: 0px;
  margin: 0px;
}

/* Header */
.header {
  width: 100%;
  position: fixed;
  display: flex;
  padding: 5px 100px 5px 100px;
  background-color: purple;
  color: white;
}

.menuButton {
  align-self: center;
  margin-left: 2%;
  font-size: 20px;
  border-radius: 5px;
  padding: 10px;
}

.menuButton a {
  font-style: bold;
  text-decoration: none;
  color: white;
  font-weight: bold;
}

.title {
  font-family: Brush Script MT, Brush Script Std, cursive;
}

.title h1 {
  font-size: 60px;
  text-decoration: aliceblue;
}

.content {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 20px 100px;
  min-height: 500px;
}

/* Footer */
.footer {
  height: 90px;
  padding: 20px 100px 0px 100px;
  background: #e3e3e3;
  font-size: 20px;
}

.footer a {
  color: #000;
}

h1 {
  font-size: 20px;
}

h2 {
  font-size: 14px;
}
</style>
