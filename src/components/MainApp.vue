<template>
    <div>
        <!-- SEARCH -->
        <div class="data">
            <input type="text" v-model="inputText">
            <button @click="getFilm">Cerca</button>
        </div>

        <!-- DATA -->
        <div class="container">
            <div class="info" v-for="(data, index) in filmSeries" :key="index">
                <!-- POSTER -->
                <img :src="`https://image.tmdb.org/t/p/w342${data.poster_path}`" :alt="data.title == null ? data.name : data.title" v-if="data.poster_path !== null" class="cover" @mouseover="hover = true" @mouseleave="hover = false">
                <img src="@/assets/img/poster.jpg" :alt="data.title == null ? data.name : data.title" v-else class="cover" @mouseover="hover = true" @mouseleave="hover = false">
                <ul v-if="hover">
                     <!-- TITOLO -->
                    <li>
                        <strong>Titolo:</strong>
                        {{ data.title == null ? data.name : data.title }}
                    </li>

                    <!-- TITOLO ORIGINALE -->
                    <li>
                        <strong>Titolo Originale:</strong>
                        {{ data.original_title == null ? data.original_name : data.original_title }}
                    </li>

                    <!-- LINGUA -->
                    <li v-if="!flag.includes(data.original_language)" class="language">
                        <strong>Lingua:</strong>
                        <img class="flag" src="@/assets/img/missing.png" :alt="data.original_language">
                    </li>
                    <li class="language" v-else>
                        <strong>Lingua:</strong>
                        <img class="flag" :src="require(`@/assets/img/${data.original_language}.png`)" :alt="data.original_language">
                    </li>

                    <!-- VOTO -->
                    <li>
                        <strong>Voto:</strong>
                        <span v-for="(stars, index) in Math.round(data.vote_average / 2)" :key="index">
                            <font-awesome-icon :icon="['fas', 'star']"/>
                        </span>
                        <span v-for="(stars, index) in 5 - Math.round(data.vote_average / 2)" :key="index">
                            <font-awesome-icon :icon="['far', 'star']"/>
                        </span>
                    </li>
                    <!-- VERVIEW -->
                    <li>
                        <strong>Overview:</strong>
                        {{data.overview}}
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
          hover: false,
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
        width: 90vw;
        margin: 0 auto;
        display: flex;
        flex-wrap: wrap;
        justify-content: center;

        .info {
            width: calc(100% / 6);
            border: 1px solid black;
            position: relative;

            .cover {
                width: 100%;
            }

            ul {
                list-style-type: none;
                position: absolute;
                top: 0;
                left: 0;
                background-color: black;
                width: 100%;
                height: 100%;

                li {
                    color: white;
                }

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
