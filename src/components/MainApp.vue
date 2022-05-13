<template>
    <div>
        <!-- SEARCH -->
        <header-app @search="getFilm"/>

        <!-- DATA -->
        <div class="container" v-if="filmSeries.length > 0">
            <div class="info" v-for="(data, index) in filmSeries" :key="index">
                <!-- POSTER -->
                <img :src="`https://image.tmdb.org/t/p/w342${data.poster_path}`" :alt="data.title == null ? data.name : data.title" v-if="data.poster_path !== null" class="cover">
                <img src="@/assets/img/poster.jpg" :alt="data.title == null ? data.name : data.title" v-else class="cover">
                <ul>
                     <!-- TITOLO -->
                    <li class="title">
                        {{ data.title == null ? data.name : data.title }}
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
                    <li class="voto">
                        <strong>Voto:</strong>
                        <span v-for="index in 5" :key="index">
                            <font-awesome-icon :icon="['fas', 'star']" v-if="index <= Math.round(data.vote_average / 2)"/>
                            <font-awesome-icon :icon="['far', 'star']" v-else/>
                        </span>
                    </li>
                    <!-- OVERVIEW -->
                    <li class="rias">
                        <strong>Overview:</strong>
                        {{data.overview}}
                    </li>

                    <!-- <li class="actor" v-if="actorList">
                        <strong>Actor:</strong>
                        <ul>
                            <li v-for="(actor, index) in actorList" :key="index">{{actor.name}}</li>
                        </ul>
                    </li>
                    <li class="genres" v-if="filteredGenres !== []">
                        <strong>Genres:</strong>
                        <span v-for="(genre, index) in filteredGenres" :key="index">{{genre.name}}</span>
                    </li> -->

                </ul>
            </div>
        </div>
        <h1 v-else>Non hai ancora effettuato un aricerca adatta</h1>
        <LoaderComponent v-if="loading"/>
    </div>
</template>

<script>
import axios from 'axios'
import HeaderApp from "./HeaderApp.vue"
import LoaderComponent from "./LoaderComponent.vue"

export default {
  name: 'MainApp',
  components: {
      HeaderApp,
      LoaderComponent
  },
  data() {
      return {
          inputText: "",
          loading: false,
          apiUrl: "https://api.themoviedb.org/3/",
          apiKey: "api_key=f09bcfe2f55857ee3aca9a399323b3ff",
          apiLenguage: "lenguage=it-IT",
          apiQuery: "",
          filmList: [],
          seriesList: [],
          filmSeries: [],
          actorList: [],
          genres: [],
          filteredGenres: [],
          flag: ['it', 'en', 'es', 'fr'],
      }
  },
  methods: {
      getMovieApi() {
          return axios.get(this.apiUrl + "search/movie?" + this.apiKey + "&query=" + this.apiQuery + "&" + this.apiLenguage);
      },
      getSeriesApi() {
          return axios.get(this.apiUrl + "search/tv?" + this.apiKey + "&query=" + this.apiQuery + "&" + this.apiLenguage);
      },
      getFilm(txt){
          this.loading = true
          this.inputText = txt
          this.apiQuery = this.inputText.replaceAll(" ", "+")
          //console.log(this.apiQuery)
          Promise.all([this.getMovieApi(), this.getSeriesApi()]).then((res) => {
              this.filmList = res[0].data.results
              this.seriesList = res[1].data.results
              this.filmSeries = this.filmList.concat(this.seriesList)
              this.loading = false
              //console.log(this.filmSeries)
          }).catch((error) => {
            console.log(error)
          })
          this.inputText = ""
      },
      /* ATTORI
      getActor(){
          axios.get(this.apiUrl + "search/movie/" + this.filmSeries.id + "/credits?" + this.apiKey + "&query=" + this.apiQuery + "&" + this.apiLenguage).then((res) => {
              this.actorList.push(res.data.cast)
              if (this.actorList.length >= 5) {
                  this.actorList.length = 5
              }
          });
      },
      //GENERI
      getGenres(){
          axios.get(this.apiUrl + "genre/movie/list?" + this.apiKey + "&" + this.apiLenguage).then((res) => {
              this.genres = res.genres
              //console.log(this.genres)
          });
      },
      filterGenres(){
          this.genres.forEach((el) => {
              if (this.seriesList.genre_ids.includes(el.id)) {
                  this.filteredGenres.push(el.name)
                  //console.log(this.filteredGenres)
              }
          })
      } */
  },
}
</script>

<style lang="scss" scoped>
    h1 {
        text-align: center;
        line-height: 50vw;
        color: white;
    }

    .data {
        display: flex;
        position: fixed;
        top: 0;
        width: 100%;
        z-index: 1000;
        justify-content: space-between;
        align-items: center;
        background-color: #141414;

        .left,
        .rigth {
            display: flex;
            align-items: center;
            color: white;

            .logo {
                width: 10em;
            }

            .logo-small {
                width: 2em;
                padding: 0.2em;
                margin-left: 1em;
                display: none;
            }

            ul {
                display: flex;
                list-style-type: none;

                li {
                    padding: 0.4em;
                    a {
                        text-decoration: none;
                         color: white;
                        font-size: 0.9em;
                    }
                }
            }

            .user {
                width: 2em;
                border-radius: 5px;
                margin: 0 1em;
            }

            input {
                padding: 10px;
                margin: 0 5px;
                border: none;
                border-radius: 10px;
            }

            button {
                padding: 10px;
                background-color: #141414;
                border: none;
                margin-right: 1em;
                cursor: pointer;
                color: white;
            }
        }
    }

    .container {
        width: 90vw;
        margin: 10em auto 0 auto;
        display: flex;
        flex-wrap: wrap;
        justify-content: center;

        .info {
            width: calc(100% / 6);
            position: relative;
            margin: 3em 0.3em;
            overflow: hidden;
            gap: 1em;
            border-radius: 5px;

            &:hover {
                transform: scale(1.4);
                z-index: 1;
                transition: ease-in-out 0.3s;

                .cover {
                    transform: scale(0.72);
                }
        
                ul {
                    opacity: 1;
                }
            }

            .cover {
                width: 100%;
                overflow: hidden;
                border-radius: 5px;
            }

            ul {
                list-style-type: none;
                position: absolute;
                top: 0;
                left: 0;
                background-color: rgba(20, 20, 20, 0.8);
                opacity: 0;
                height: 100%;
                border-radius: 5px;
                // box-shadow: 0px 0px 10px 5px #000000;
                display: flex;
                flex-flow: column nowrap;
                overflow-y: auto;
                text-align: center;

                &::-webkit-scrollbar {
                    width: 0;
                }

                li {
                    color: white;
                    padding: 1em 1em;
                }

                .title {
                    font-size: 1.5em;
                }

                .rias {
                    margin: 1em 0;
                }

                .language {
                    display: flex;
                    justify-content: center;

                    .flag {
                        width: 1.5em;
                    }
                }


            }
        }
    }

    @media screen and (max-width: 1440px){
        .container .info {
            width: calc((100% / 4) - 2em);
        }
    }

    @media screen and (max-width: 768px){
        .container .info {
            width: calc((100% / 2) - 2em);
        }
        .data .left ul,
        .data .rigth .bell,
        .data .rigth .user {
            display: none;
        }
    }

    @media screen and (max-width: 375px){
        .container .info {
            width: 100%;
            margin: 3em;
        }
        .data .left .logo {
            display: none;
        }
        .data .left .logo-small {
            display: block;
        }
    }
</style>
