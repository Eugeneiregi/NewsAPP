<template>


  <v-app light>

  <!--The SideMenu Component go here-->  
  <SideMenu :drawer="drawer"  :api_key="api_key" @selectsource="setResource" ></SideMenu>

  <v-toolbar fixed app light clipped-left color="primary" class="elevation-2">
    <v-toolbar-side-icon @click="drawer = !drawer"  class="white--text"></v-toolbar-side-icon>
    <v-toolbar-title class="white--text">News App <span>By Eugene </span></v-toolbar-title>
  </v-toolbar>

  <v-content>
    <v-container fluid>

      <!--The MainContent Component go here-->  
      <MainContent :articles="articles"></MainContent>

    </v-container>
   </v-content>
   <v-footer class="secondary" app>
      <v-layout row wrap align-center>
        <v-flex xs12>
          <div class="white--text ml-3">
            Made with
            <v-icon class="red--text">favorite</v-icon>
            by <a class="white--text" href="https://vuetifyjs.com" target="_blank">Vuetify</a>
            and <a class="white--text" href="https://github.com/Eugeneiregi" target="_blank">Eugene Iregi</a>
          </div>
        </v-flex>
      </v-layout>
    </v-footer>
  </v-app>


</template>

<script>

import axios from 'axios' //importing the axios a HTTP library to connects the app with the API
import SideMenu from './components/SideMenu.vue' // import the SideMenu component
import MainContent from './components/MainContent.vue' // import the MainContent component


export default {

  components: {
      SideMenu,
      MainContent
  },

  data() {
    return {
      drawer: true, // true to show/hide the side navigation drawer 
      api_key:'40238d06fc8d4a2c825877e15f8d9e03',
      articles: [],
      errors: [] 
    }
  },


  created () {
    axios.get('https://newsapi.org/v2/top-headlines?sources=techcrunch&apiKey='+this.api_key)
      .then(response => {
        this.articles = response.data.articles
        console.log('data:')
        console.log(response.data.articles)
      .catch(e => {
        this.errors.push(e)
      })
    })
  },


  methods: {
     
      setResource(source){

        axios.get('https://newsapi.org/v2/top-headlines?sources='+source+'&apiKey='+this.api_key)
        .then(response => {
          //this.articles = response.data.articles
          this.articles = response.data.articles
          console.log('Source Articles:')
          console.log(response.data.articles) // This will give you access to the full object
        })
        .catch(e => {
          this.errors.push(e)
        })

      }


   }

  }
</script>