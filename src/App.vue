<template>
  <div id="app">
    <font-awesome-icon icon="user-secret" />
    <div class="heading">
      <div class="title">Giphy Search</div>
      <input type="search" v-model="searchTerm" />
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
.title {
  font-size: 60px;
  margin: 30px 0;
}
input {
  padding: 10px 10px 10px 20px;
  margin-bottom: 20px;
  font-size: 28px;
  border: 1px solid lightblue;
  border-radius: 5px;
  outline: none;
  color: darkcyan;
}
input::selection {
  background-color: darkmagenta;
  color: white;
}
hr {
  background: lightgray;
  opacity: 0.3;
}
.btn-search {
  cursor: pointer;
  padding: 10px 30px;
  display: block;
  margin: 0 auto;
  border: none;
  font-size: 24px;
  border: 1px solid darkcyan;
  background: transparent;
}
.btn-search:hover,
.btn-search:active {
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
