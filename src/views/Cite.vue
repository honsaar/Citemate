<template>
  <div class="about">
    <div class="hero">
      <div class="container">

        <!-- use this layout if the bibliography is empty, otherwise change the wording and layout of the top -->

        <div id="stages">
          <!-- 1: choose your reference style; 2: choose the type of resource you're citing; 3: search -->

        </div>

        <div id="refStyle">
          <h1 class="brand">First, choose your reference style</h1>
          <p class="subtitle">Don't worry, you can change your reference style at any point.</p>
          <br />
          <b-btn variant="primary" class="secButt" @click="chooseStyle('Harvard')">Harvard</b-btn>
          <b-btn variant="primary" class="secButt" @click="chooseStyle('MLA')">MLA</b-btn>
          <b-btn variant="primary" class="secButt" @click="chooseStyle('Chicago')">Chicago</b-btn>
          <b-btn variant="primary" class="secButt" @click="chooseStyle('Vancouver')">Vancouver</b-btn>
          <br />
          <span class="styleList" @click="showMore = !showMore">The style I want isn't listed</span>
          <br />
          <div v-if="showMore">
            <p>Show more styles here</p>
          </div>
          <br><br>
        </div>
      </div>
    </div>
    <div class="content" style="background: white;padding-top:5em;">
      <br />
      <br />
      <br />
      <div style="text-align: center;">
         <!-- <b-btn variant="primary" class="primeButt" @click="cite()">Test cite, yo</b-btn> -->
         <div id="emptyState" v-if="bibliography.length < 1">
          <h2 class="brand">Your bibliography is currently empty</h2>
          <p>Start adding references by choosing a reference style</p>
          </div>
        </div>
      <br />
      <br />
      <div class="results">
        <div class="resultCard" v-for="(paper, key) in results" :key="key">
          <p>
            <strong>{{paper.title}}</strong>
          </p>
          <p>
            <span v-for="(author, index) in paper.authors" :key="index">
            {{author.given}} {{author.family}}<span v-if="index < paper.authors.length-1">,</span>
            </span>
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

//FIXME: (not a fix, just so there's a highlight in Code) use this list for guides and a list of different styles: http://www.citethisforme.com/guides

// @ is an alias to /src
import axios from "axios";

export default {
  name: "home",
  components: {},
  data: function() {
    return {
      bibliography: [],
      adding: false,
      results: [],
      query:
        "Is+chronic+breathlessness+less+recognised+and+treated+compared+with+chronic+pain",
      showMore: false
    };
  },
  methods: {
    cite() {
      console.log("Loading response...");
      axios
        .get("https://api.crossref.org/works?query=" + this.query)
        .then(response => {
          console.log("Loaded!");
          for (let i = 0; i < response.data.message.items.length; i++) {
            var search = response.data.message.items[i];
            var tempResult = {
              title: "", //title[0]
              doi: "", //DOI
              authors: [], //author[]
              published: { year: "", month: "", day: "" }, //created
              issue: "", //issue
              publisher: "", //publisher
              link: "", //url
              volume: "", //volume
              journal: "", //container-title[0]
              pages: "" //page
            };

            if (search.title != null || search.title != undefined) {
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
            this.results.push(tempResult);
            console.log(search);
          }
          console.log(this.results);
        });
    },
    book() {
      console.log("Loading response for book libraries...");
    },
    styleList() {
      console.log("Loading list of other styles...");
    },
    chooseStyle(style) {
      console.log("Current reference style is " + style);
    }
  }
};
</script>

<style>
.styleList {
  font-size: 0.8em;
  text-decoration: underline;
  cursor: pointer;
}

.styleList:hover {
  color: #ff8552;
}
</style>
