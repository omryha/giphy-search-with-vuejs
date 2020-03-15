<template>
  <div id="app">
    <h1>Giphy Search</h1>
    <input type="text" v-model="searchTerm" />
    <div class="btn-container">
      <button class="btn-search" @click="getGifs()">Search</button>
    </div>
    <hr />
    <div class="gif-container">
      <img v-for="gif in gifs" :src="gif" :key="gif.id" />
    </div>
  </div>
</template>

<script>
export default {
  title: "Giphy Search",
  data() {
    return {
      searchTerm: "",
      gifs: []
    };
  },
  methods: {
    getGifs() {
      let apiKey = "dc6zaTOxFJmzC";
      let searchEndPoint = "https://api.giphy.com/v1/gifs/search?";
      let limit = 10;
      let url = `${searchEndPoint}&api_key=${apiKey}&q=${this.searchTerm}&limit=${limit}`;

      fetch(url)
        .then(response => {
          return response.json();
        })
        .then(json => {
          this.buildGifs(json);
        })
        .catch(error => console.log(error));
    },
    buildGifs(json) {
      this.gifs = json.data
        .map(gif => gif.id)
        .map(gifId => {
          return `https://media.giphy.com/media/${gifId}/giphy.gif`;
        });
    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  font-family: "Coda";
}
input {
  padding: 10px;
  margin-bottom: 20px;
  font-size: 22px;
  border: 1px solid lightblue;
  border-radius: 5px;
  outline: none;
  color: darkcyan;
}
.btn-search {
  cursor: pointer;
  padding: 5px 20px;
  display: block;
  margin: 0 auto;
  border: 1px solid darkcyan;
  border-radius: 5px;
  background-color: transparent;
}
.btn-search:hover {
  background-color: lightcyan;
}
.gif-container {
  margin-top: 30px;
  display: flex;
  flex-direction: column;
  align-items: center;
}
img {
  margin: 10px 5px;
}
</style>
