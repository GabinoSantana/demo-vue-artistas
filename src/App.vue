<template lang="pug">
  #app
    img(src='https://gabinosantana.github.io/demo-vue-artistas/dist/logo.png')
    h1 Lista de Artistas
    select(v-model="selectedCountry")
      option(v-for="country in countries" v-bind:value="country.value") {{country.name}}
    spinner(v-show="loading")
    ul
      artist(v-for="artist in artists" v-bind:artist="artist" :key="artist.mbid")
</template>

<script>
import Artist from './components/artist.vue'
import Spinner from './components/spinner.vue'
import getArtists from './api'
export default {
  name: 'app',
  data () {
    return {
      selectedCountry:'argentina',
      artists:[],
      countries:[
        {'name': 'Argentina', value:'argentina'},
        {'name': 'Colombia', value:'colombia'},
        {'name': 'España', value:'spain'}
      ],
      loading: true
    }
  },
  components:{
    Artist,
    Spinner
  },
  methods:{
    refreshArtists(){
      this.artists =[]
      this.loading = true
      const self = this
      getArtists(this.selectedCountry).then(
        function (artists){
          self.loading = false
          self.artists = artists
        }
      )
    }
  },
  mounted: function(){
    this.refreshArtists()
  },
  watch:{
    selectedCountry:function(){
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
  font-weight normal

ul
  list-style-type none
  padding 0

li
  display inline-block
  margin 0 10px

a
  color #42b983
</style>
