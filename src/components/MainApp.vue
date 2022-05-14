<template>
    <div>
        <!-- SEARCH -->
        <header-app @search="getFilm"/>
        <!-- PRINT -->
        <main>
            <grid-component :list="filmList" :categoria="movieTitle" :loading="loading" :research="getFilm"/>
            <grid-component :list="seriesList" :categoria="seriesTitle" :loading="loading" :research="getFilm"/>
        </main>
    </div>
</template>

<script>
import axios from 'axios'
import HeaderApp from "./HeaderApp.vue"
import GridComponent from "./GridComponent.vue"

export default {
  name: 'MainApp',
  components: {
      HeaderApp,
      GridComponent
  },
  data() {
      return {
          loading: false,
          apiUrl: "https://api.themoviedb.org/3/search/",
          apiKey: "api_key=f09bcfe2f55857ee3aca9a399323b3ff",
          apiLenguage: "lenguage=it-IT",
          apiQuery: "",
          filmList: [],
          movieTitle: "",
          seriesList: [],
          seriesTitle: "",
      }
  },
  methods: {
      getMovieApi() {
          return axios.get(this.apiUrl + "movie?"  + this.apiKey + "&query=" + this.apiQuery + "&" + this.apiLenguage);
      },
      getSeriesApi() {
          return axios.get(this.apiUrl + "tv?" + this.apiKey + "&query=" + this.apiQuery + "&" + this.apiLenguage);
      },
      getFilm(txt){
          this.movieTitle = "Movies"
          this.seriesTitle = "Series"
          this.loading = true
          this.apiQuery = txt.replaceAll(" ", "+")
          Promise.all([this.getMovieApi(), this.getSeriesApi()]).then((res) => {
              this.filmList = res[0].data.results
              this.seriesList = res[1].data.results
              this.loading = false
          }).catch((error) => {
            console.log(error)
            this.loading = false
          })
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

</style>
