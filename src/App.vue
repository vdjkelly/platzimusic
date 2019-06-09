<template lang="pug">
  #app
    img(src='./assets/logo.png')
    h1 Platzi Music
    select(v-model="selectContry")
      option(v-for="country in countrys" v-bind:value="country.value") {{ country.name }}
    spinner(v-show="loading")
    ul
      Artist(v-for="artist in artists" :artist="artist")
</template>

<script>
import getArtists from './api'
import Artist from './components/Artists.vue'
import Spinner from './components/Spinner.vue'
export default {
  name: 'app',
  components: {
    Artist,
    Spinner
  },
  data () {
    return {
      artists: [],
      countrys: [
        {
          name: 'Colombia', value: 'colombia',
        },
        {
          name: 'Venezuela', value: 'venezuela',
        },
        {
          name: 'Argentina', value: 'argentina',
        }
      ],
      selectContry: 'venezuela',
      loading: false
    }
  },
  methods: {
    refresArtists () {
      const self = this
      self.loading = true
      getArtists(this.selectContry).then( function (artists) {
        self.artists = artists
      }).finally(function (){
        self.loading = false
      })
    }
  },
  mounted() {
    this.refresArtists()
  },

  watch: {
    selectContry: function () {
      this.refresArtists()
    }
  },
}
</script>

<style lang="stylus">
#app
  font-family 'Avenir', Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  text-align center
  color #2c3e50
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
