<template>
  <div id="app">
    <div class="heading">
      <div class="title">GIF Search</div>
      <div class="openingGif-container">
        <img :src="openingGif" :key="openingGif.id" loading="lazy" />
      </div>
      <div class="actions">
        <input
          type="search"
          placeholder="Enter keyword 🐾"
          v-on:keyup.enter="getGifs()"
          v-model="searchTerm"
        />
        <label for="weirdness">
          Weirdness Level (for a special GIF):
          <span>{{this.weirdness}}</span>
        </label>
        <input type="range" id="weirdness" min="0" max="10" step="1" v-model="weirdness" />
      </div>
      <div class="btn-container">
        <button class="btn-search" @click="getGifs()">Search for GIFS 🐱</button>
        <button class="btn-search translate" @click="translateToGif()">Special GIF ✨</button>
        <button class="btn-search trending" @click="getGifs(true)">Trending GIFS 🔥</button>
      </div>
    </div>

    <hr v-if="gifs.length>0" />
    <div class="gif-container">
      <img v-for="gif in gifs" :src="gif" :key="gif.id" loading="lazy" />
      <img v-if="translationGif" :src="translationGif" loading="lazy" />
    </div>
  </div>
</template>

<script>
export default {
  title: "Giphy Search",
  data() {
    return {
      apiKey: "onYkOD8hKuuHkmqEvyINdV1PKXQMjaSk",
      openingGif: "",
      searchTerm: "",
      weirdness: 0,
      lang: "en",
      gifs: [],
      translationGif: ""
    };
  },
  methods: {
    getGifs(trending = false) {
      let searchEndPoint = "https://api.giphy.com/v1/gifs/search?";
      let url = "";
      let trendingEndPoint = "";
      let limit = "";

      if (trending) {
        trendingEndPoint = "https://api.giphy.com/v1/gifs/trending?";
        url = `${trendingEndPoint}api_key=${this.apiKey}`;
        fetch(url)
          .then(response => {
            return response.json();
          })
          .then(json => {
            this.buildGifs(json);
          })
          .catch(error => console.log(error));
      }

      this.translationGif = "";

      if (!trending) {
        url = `${searchEndPoint}&api_key=${this.apiKey}&q=${this.searchTerm}&limit=${limit}&offset=0&rating=G&lang=${this.lang}`;
        if (this.searchTerm) {
          this.gifs = [];
          fetch(url)
            .then(response => {
              return response.json();
            })
            .then(json => {
              this.buildGifs(json);
            })
            .catch(error => console.log(error));
        } else {
          alert("Can't search none");
        }
      }
    },
    buildGifs(json) {
      this.gifs = json.data
        .map(gif => gif.id)
        .map(gifId => {
          return `https://media.giphy.com/media/${gifId}/giphy.gif`;
        });
    },
    getOpeningGif() {
      let openingGifUrl =
        "https://api.giphy.com/v1/gifs/random?api_key=onYkOD8hKuuHkmqEvyINdV1PKXQMjaSk&tag=&rating=G";

      fetch(openingGifUrl)
        .then(response => {
          return response.json();
        })
        .then(json => {
          this.openingGif = this.getSingleGif(json);
        })
        .catch(error => console.log(error));
    },
    translateToGif() {
      let translationEndPoint = "https://api.giphy.com/v1/stickers/translate?";
      let translationUrl = `${translationEndPoint}&api_key=${this.apiKey}&s=${this.searchTerm}&weirdness=${this.weirdness}`;

      this.gifs = [];

      if (this.searchTerm) {
        fetch(translationUrl)
          .then(response => {
            return response.json();
          })
          .then(json => {
            this.translationGif = this.getSingleGif(json);
          })
          .catch(error => console.log(error));
      } else {
        alert("Can't search none");
      }
    },
    getSingleGif(json) {
      return `https://media.giphy.com/media/${json.data.id}/giphy.gif`;
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
::selection {
  background-color: darkmagenta;
  color: white;
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
  padding: 0 30px 0px;
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
input[type="range"] {
  margin: 0;
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
  margin: 20px auto;
  border: none;
  font-size: 22px;
  border: 1px solid darkcyan;
  background: transparent;
  width: 250px;
}
.translate {
  border: 1px solid lightcoral;
  font-size: 22px;
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
label > span {
  font-weight: bold;
}
</style>
