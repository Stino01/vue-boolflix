<template>
    <div>     
        <div class="data">
            <input type="text" v-model="inputText">
            <button @click="getFilm">Cerca</button>
        </div>
        <div class="container">
            <div class="info" v-for="(data, index) in filmSeries" :key="index">
                <ul>
                    <li>
                        <strong>Titolo:</strong>
                        {{ data.title == null ? data.name : data.title }}
                    </li>
                    <li>
                        <strong>Titolo Originale:</strong>
                        {{ data.original_title == null ? data.original_name : data.original_title }}
                    </li>
                    <li v-if="!flag.includes(data.original_language)">
                        <strong>Lingua:</strong>
                        {{data.original_language}}
                    </li>
                    <li class="language" v-else>
                        <strong>Lingua:</strong>
                        <img class="flag" :src="require(`@/assets/img/${data.original_language}.png`)" :alt="data.original_language">
                    </li>
                    <li>
                        <strong>Voto:</strong>
                        {{data.vote_average}}
                    </li>
                </ul>
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
          apiUrl: "https://api.themoviedb.org/3/search/",
          apiKey: "api_key=f09bcfe2f55857ee3aca9a399323b3ff",
          apiLenguage: "lenguage=it-IT",
          apiQuery: "",
          filmList: [],
          seriesList: [],
          filmSeries: [],
          flag: ['it', 'en', 'es', 'fr'],
      }
  },
  methods: {
      getMovieApi() {
          return axios.get(this.apiUrl + "movie?" + this.apiKey + "&query=" + this.apiQuery + "&" + this.apiLenguage);
      },
      getSeriesApi() {
          return axios.get(this.apiUrl + "tv?" + this.apiKey + "&query=" + this.apiQuery + "&" + this.apiLenguage);
      },
      getFilm(){
          this.apiQuery = this.inputText.replaceAll(" ", "+")
          //console.log(this.apiQuery)
          Promise.all([this.getMovieApi(), this.getSeriesApi()]).then((res) => {
              this.filmList = res[0].data.results
              this.seriesList = res[1].data.results
              this.filmSeries = this.filmList.concat(this.seriesList)
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

            ul {
                list-style-type: none;

                .language {
                    display: flex;

                    .flag {
                        width: 1.5em;
                    }
                }
            }
        }
    }
</style>
