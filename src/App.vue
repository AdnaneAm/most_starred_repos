<template>
  <div id="app">
    <Header @usr-search="updateSearchTerm"/>
    <Repos ref="repos" :searchTerm="searchTerm"/>
  </div>
</template>
<script>
import Repos from './components/Repos.vue'
import Header from './components/Header.vue'

export default {
  data(){
    return {
      searchTerm:''
    }
  },
  methods:{
    updateSearchTerm(value){
      this.searchTerm=value;
    }
  },
  components:{
    Repos,Header
    }
}

</script>

<style lang="scss">
// General styles and resets
h1,h2,h3,h4,h5,p{
  padding:0 !important;
  margin:0 !important;
}
body{
  background:#f9feff;
  height:100%;
}
html{
  overflow-x:hidden;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
