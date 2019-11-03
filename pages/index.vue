<template>
  <div class="container">
    <div>
      <logo />
      <div class="divright">
        <img v-bind:src="image" />
      </div>
      <div class="divleft">
        <h2 class="subtitle1">MuGIF</h2>
        <h2 class="subtitle2">The Musical Gif Generator</h2>
        <input v-on:change="inputUpdate" v-bind:value="inputValue" placeholder="Type Song Name" />
        <button class="button" v-on:click="updater">Search</button>
      </div>
      <!-- <div>
        <img v-bind:src="gifDisplay" />
      </div>-->
      <iframe
        v-bind:src="songUrl"
        width="300"
        height="80"
        frameborder="0"
        allowtransparency="true"
        allow="encrypted-media"
      ></iframe>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Logo from "~/components/Logo.vue";
import { objectExpression } from "babel-types";

const response = { test: "hello" };

export default {
  data() {
    return {
      image: "hello",
      songUrl: null,
      inputValue: "",
      gifDisplay: "happy",
      // Takes in array of .gifs search queries and displays a search
      returnGif: () => {
        axios.get(
          "api.giphy.com/v1/gifs/search?q=happy&api_key=QPYjJIZRtHN9y24gOEi1s4Nk1Y3zUV9g&limit=5"
        );
        console
          .log("This is getting the gif")
          .then(response => {
            console.log("THIS IS DISPLAY OF GIFS");
            console.log(gifDisplay);
            this.gifDisplay = response.data.fixed_width_downsampled_url;
          })
          .catch(e => {
            console.log(e, "ERROR in gifDisplay");
          });
      },
      updater: () => {
        axios
          // .get(
          //   "https://www.googleapis.com/youtube/v3/search?key=AIzaSyAUyXiXkqQv2oaXmtZjkyIwLhMvs1Jp8xo&q=" +
          //     this.inputValue +
          //     "&part=id,snippet"
          // )
          //GET API KEY SPOTIFY
          //MAKE SPOTIFY API CALL https://api.spotify.com/v1/search?q=hotline&type=track&YOUR_API_KEY=...
          //CHANGE THE EMBED TO BE A SONG EMBED .. NOT AN ALBUM EMBED

          //.get("https://api.spotify.com/v1/search?q=hotline&type=track&")
          .get("https://api.spotify.com/v1/search", {
            headers: {
              Authorization:
                "Bearer BQC3nzVR6vQfGRod_sgy1g6CdzqZM6YS8g-BSsRVcUR9EYMAFg16stX2wU1ZbWbkiuajpOMzVOy9sTKmco-pZM586NKaxEv8PnwZO74z1mjg4u9SYGkHnygym8wx4u_raqFV2QyKdmwwzX6zUhIZKKb2MlIcmvBD9Q"
            },
            params: {
              q: encodeURI(this.inputValue),
              type: "track"
              //"artist":
            }
          })
          .then(response => {
            // returns with requested track
            this.songUrl =
              "https://open.spotify.com/embed/track/" +
              response.data.tracks.items[0].id;
          })
          .catch(e => {
            console.log(e, "ERROR");
          });
      },
      inputUpdate: e => {
        this.inputValue = e.target.value;
      }
    };
  },
  mounted() {
    axios
      .get(
        "http://api.giphy.com/v1/gifs/random?api_key=QPYjJIZRtHN9y24gOEi1s4Nk1Y3zUV9g"
      )
      .then(response => {
        console.log(response);
        this.image = response.data.data.fixed_width_downsampled_url;
      });
  },
  components: {
    Logo
  }
};
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: block;
  padding-top: 15px;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: "Quicksand", "Source Sans Pro", -apple-system, BlinkMacSystemFont,
    "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.button {
  margin: 15px;
}
</style>
