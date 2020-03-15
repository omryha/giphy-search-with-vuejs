<template>
  <div id="app">
    <div class="heading">
      <h1>Giphy Search</h1>
      <input type="text" v-model="searchTerm" />
      <div class="btn-container">
        <button class="btn-search" @click="getGifs()">Search</button>
      </div>
    </div>

    <hr />
    <div class="gif-container">
      <img v-for="gif in gifs" :src="gif" :key="gif.id" />
      <!--     <masonry :col="10">
      </masonry>-->
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
      let apiKey = "onYkOD8hKuuHkmqEvyINdV1PKXQMjaSk";
      let searchEndPoint = "https://api.giphy.com/v1/gifs/search?";
      let limit = 25;
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
* {
  font-family: "Kreon", serif;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.heading {
  display: flex;
  flex-direction: column;
  place-content: center;
  place-items: center;
  margin: 10px 30px;
}
input {
  padding: 10px;
  margin-bottom: 20px;
  font-size: 22px;
  border: 1px solid lightblue;
  border-radius: 5px;
  outline: none;
  color: darkcyan;
  background-color: blue;
}
.btn-search {
  cursor: pointer;
  padding: 5px 20px;
  display: block;
  margin: 0 auto;
  border: 1px solid darkcyan;
  border-radius: 5px;
  background-color: transparent;
  font-size: 18px;
}
.btn-search:hover {
  background-color: lightcyan;
  font-weight: bold;
}
hr {
  margin: 20px 0;
}
.gif-container {
  display: flex;
  flex-direction: column;
  flex-flow: row wrap;
  place-content: center;
}
img {
  width: 20em;
  height: 15em;
  margin: 5px;
  box-shadow: 1px 1px 10px 1px black;
}
</style>
