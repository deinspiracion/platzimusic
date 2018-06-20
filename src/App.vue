<template lang="pug">
  #app
    img(src='https://deinspiracion.github.io/platzimusic/dist/logo.png')
    h1 Platzimusc
    select(v-model="selectedCountry")
      option(v-for="country in countries" :value="country.value") {{country.name}}
    spinner(v-show="loading")
    ul
      artist(v-for="artist in artists" v-bind:artist="artist" v-bind:key="artist.mbid")
    

</template>

<script>
import Artist from './components/Artist'
import getArtists from '../api'
import spinner from './components/loading.vue'
import { logicalExpression } from 'babel-types';
export default {
  name: 'app',
  data () {
    return {
      artists:[],
      countries:[
        {name:'Argentina',value:"argentina"},
        {name:'Colombia',value:"colombia"},
        {name:'España',value:"spain"}
      ],
      selectedCountry:'argentina',
      loading:true
    }
  },
  components:{
    Artist,
    spinner
  },
  methods:{
    refreshArtist(){
       const self = this
       this.loading=true;
        this.artists=[];
        getArtists(this.selectedCountry).then( function(artists){
          self.loading=false
          self.artists= artists
        } )
    }
  },
  mounted:function(){
    console.log("mount");
    
    this.refreshArtist()
  },
  watch:{
    
    
    selectedCountry:function(){
      console.log("watch");
      this.refreshArtist()
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
  color red !important
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
