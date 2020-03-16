<template>
  <div id="app">
    <div class="heading">
      <div class="title">Giphy Search</div>
      <div class="openingGif-container">
        <img :src="openingGif" :key="openingGif.id" loading="lazy" />
      </div>
      <div class="actions">
        <input
          type="search"
          placeholder="Search a GIF 🐾"
          v-on:keyup.enter="getGifs()"
          v-model="searchTerm"
        />
        <!--         <select v-model="lang">
          <option disabled value>Select Language</option>
          <option value="en">English</option>
          <option value="hi">Hebrew</option>
          <option value="ar">Arabic</option>
        </select>-->
      </div>
      <div class="btn-container">
        <button class="btn-search" @click="getGifs()">Search</button>
      </div>
    </div>

    <hr />
    <div class="gif-container">
      <img v-for="gif in gifs" :src="gif" :key="gif.id" loading="lazy" />
    </div>
  </div>
</template>

<script>
export default {
  title: "Giphy Search",
  data() {
    return {
      openingGif: "",
      searchTerm: "",
      lang: "en",
      gifs: []
    };
  },
  methods: {
    getGifs() {
      let apiKey = "onYkOD8hKuuHkmqEvyINdV1PKXQMjaSk";
      let searchEndPoint = "https://api.giphy.com/v1/gifs/search?";
      let limit = "";
      let url = `${searchEndPoint}&api_key=${apiKey}&q=${this.searchTerm}&limit=${limit}&offset=0&rating=G&lang=${this.lang}`;

      fetch(url)
        .then(response => {
          return response.json();
        })
        .then(json => {
          this.buildGifs(json);
        })
        .catch(error => console.log(error));
    },
    getOpeningGif() {
      let openingGifUrl =
        "https://api.giphy.com/v1/gifs/random?api_key=onYkOD8hKuuHkmqEvyINdV1PKXQMjaSk&tag=&rating=G";

      fetch(openingGifUrl)
        .then(response => {
          return response.json();
        })
        .then(json => {
          this.buildOpeningGif(json);
        })
        .catch(error => console.log(error));
    },
    buildOpeningGif(json) {
      console.log(json.data.id);
      this.openingGif = `https://media.giphy.com/media/${json.data.id}/giphy.gif`;
    },
    buildGifs(json) {
      this.gifs = json.data
        .map(gif => gif.id)
        .map(gifId => {
          return `https://media.giphy.com/media/${gifId}/giphy.gif`;
        });
    }
  },
  beforeMount: function() {
    this.getOpeningGif();
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
.actions {
  display: flex;
  flex-direction: column;
  padding: 0 30px 30px;
}
select {
  height: 50px;
  text-align: center;
  font-size: 23px;
  padding-left: 10px;
  border-radius: 5px;
  border: 1px solid lightsteelblue;
}
.openingGif-container {
  margin-bottom: 30px;
}
.openingGif-container img {
  box-shadow: none;
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
