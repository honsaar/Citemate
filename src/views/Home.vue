<template>
  <div class="home">
    <div class="container">
    <p>AAA</p>
    <b-btn variant="primary" @click="cite()">Cite Journal</b-btn>
    <br/><br/>
    <div class="results">
      <div v-for="(paper, key) in results" :key="key">
        <h2><strong>Title:</strong> {{paper.title}}</h2>
        <p><strong>Authors:</strong> {{paper.authors}}</p>
        <p><strong>Published By:</strong> {{paper.publisher}}</p>
        <p><strong>Appeared In:</strong> {{paper.journal}}</p>
        <br/>
      </div>
    </div>
      </div>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from 'axios'

export default {
  name: 'home',
  components: {

  },
  data: function(){
    return {
    results: []
    }
  },
methods: {
    cite() {
      console.log("Loading response...");
      axios.get("https://api.crossref.org/works?query=Is+chronic+breathlessness+lessrecognised+and+treated+compared+with+chronic+pain").then((response) => {
          console.log("Loaded!");
          
          
          for(let i = 0; i < response.data.message.items.length; i++){
            var search = response.data.message.items[i];
            var tempResult = {
              title: '', //title[0]
              doi: '', //DOI
              authors: [], //author[]
              published: {year: '', month: '', day: ''}, //created
              issue: '', //issue
              publisher: '', //publisher
              link: '', //url
              volume: '', //volume
              journal: '', //container-title[0]
              pages: '' //page
            }
            console.log(search);
            if(search.title != null || search.title != undefined){
            tempResult.title = search.title[0];
            }
            tempResult.doi = search.DOI;
            tempResult.authors = search.author;
            tempResult.published.year = search.created["date-parts"][0][0];
            tempResult.published.month = search.created["date-parts"][0][1];
            tempResult.published.day = search.created["date-parts"][0][2];
            tempResult.issue = search.issue;
            tempResult.publisher = search.publisher;
            tempResult.link = search.URL;
            tempResult.volume = search.volume;
            tempResult.journal = search["container-title"][0];
            tempResult.pages = search.page;
            
            console.log(tempResult);
            this.results.push(tempResult);
          }
          console.log(this.results);
          })
      
    },
    book(){
      console.log("Loading response for book libraries...");
    }
  }
}
</script>
