<template>
    <div>     
        <div class="data">
            <input type="text" v-model="inputText">
            <button @click="getFilm">Cerca</button>
        </div>
        <div class="container">
            <div class="info" v-for="(data, index) in filmList" :key="index">
                <p>Titolo: {{data.title}}</p>
                <p>Titolo Originale: {{data.original_title}}</p>
                <p>Lingua: {{data.original_language}}</p>
                <p>Voto: {{data.vote_average}}</p>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'MainApp',
  data() {
      return {
          inputText: "",
          apiUrl: "https://api.themoviedb.org/3/search/movie",
          apiKey: "api_key=f09bcfe2f55857ee3aca9a399323b3ff",
          apiLenguage: "lenguage=it-IT",
          apiQuery: "",
          filmList: [],
      }
  },
  methods: {
      getFilm(){
          this.apiQuery = this.inputText.replaceAll(" ", "+")
          //console.log(this.apiQuery)
          axios.get(this.apiUrl + "?" + this.apiKey + "&query=" + this.apiQuery + "&" + this.apiLenguage).then((res) => {
              this.filmList = res.data.results
              //console.log(this.filmList)
          }).catch((error) => {
            console.log(error)
          })
          this.inputText = ""
      },
  },
}
</script>

<style lang="scss" scoped>
    .data {
        display: flex;
        justify-content: center;
        padding: 2em 0;

        input,
        button {
            padding: 10px;
            margin: 0 5px;
            border-radius: 10px;
        }
    }

    .container {
        width: 80vw;
        margin: 0 auto;
        display: flex;
        flex-wrap: wrap;
        justify-content: center;
        gap: 2em;

        .info {
            width: calc((100% - 4em) / 3);
            padding: 1em;
            border: 1px solid black;
            border-radius: 10px;
        }
    }
</style>