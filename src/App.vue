<template lang="pug">
  #app
    img(src='./assets/logo.png')
    h2 Mi Music
    select(v-model="selectedCountry")
      option(v-for="Country in Countries" :value="Country.value") {{ Country.name }}
    spinner(v-show="loading")
    ul 
      artist(v-for="artist in artists" :artist="artist" :key="artist.mbid")
</template>

<script>
import getArtists from './api';
import Spinner from "./components/Spinner.vue";
import Artist from './components/Artist.vue'
export default {
  name: 'app',
  data () {
    return {
      artists: [],
      Countries: [
        {name: 'Argentina', value:'argentina'},
        {name: 'Colombia US', value:'colombia'},
        {name: 'España', value:'spain'},
      ],
      selectedCountry: 'argentina',
      loading: true
    }
  },
  components: {
    Artist,
    Spinner
  },
  methods: {
  refreshArtists(){
    this.loading= true
    const self = this
    this.artists = []
    getArtists(this.selectedCountry)
    .then(function (artists) {
      self.artists = artists
      self.loading=false
    })
    }
  },
  mounted:  function () {
     this.refreshArtists()
  },
  watch:{
    selectedCountry() {
      this.refreshArtists()
    }
  }
}
</script>

<style lang="stylus">
#app
    font-family 'Avenir', Helvetica, Arial, sans-serif
    -webkit-font-smoothing antialiased
    -moz-osx-font-smoothing grayscale
    text-align center
    color red
    margin-top 60px

h1, h2
    font-weight norma
    color black

ul
    list-style-type none
    padding 0

li
    display inline-block
    margin 0 10px
    color black

a
    color #42b983
</style>
