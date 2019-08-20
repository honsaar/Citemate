<template>
  <div class="home">
    <!-- hero/cta biz -->
    <div class="hero">
      <div class="container">
        <h1 class="brand">Citations simplified.</h1>
        <p class="subtitle">
          Stop doing your references by hand. Create citations, reference lists and bibliographies automatically and
          <span
            class="tagline"
          >focus on what matters</span>.
        </p>
        <br/>
        <!-- TODO: Too similar to Citationsy, change it up. The irony! -->
         <b-btn variant="primary" class="primeButt">Create a Bibliography</b-btn>
      </div>
    </div>

    <div class="content" style="background: white;">
      <div class="container">
        <div id="features">
          <b-row>
            <b-col>
              <p><img src="../assets/ref.svg"/></p>
              <p class=""><strong>Your format, your way</strong></p>
              <p>Format your references for Harvard, MLA and Chicago styles plus many others</p>
            </b-col>
            <b-col>
              <p><img src="../assets/priv.svg"/></p>
              <p class=""><strong>Truly private</strong></p>
              <p>Your references are yours, and yours alone</p>
            </b-col>
            <b-col>
              <p><img src="../assets/bibl.svg"/></p>
              <p class=""><strong>Robust and ready</strong></p>
              <p>Build your bibliography and reference in your text</p>
            </b-col>
          </b-row>
        </div>
          <br><br><br>
        <p style="text-align: center;"><b-btn variant="primary" class="primeButt" @click="cite()">Test cite, yo</b-btn></p>
        <br />
        <br />
        <div class="results">
          <div class="resultCard" v-for="(paper, key) in results" :key="key">
            <p>
              <strong>{{paper.title}}</strong>
            </p>
            <p>
              <span v-for="(author, index) in paper.authors" :key="index">
                {{author.given}} {{author.family}}
                <span v-if="index < paper.authors.length-1">,</span>
              </span>
            </p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<style scoped>
.hero {
  text-align: center;
  min-height: 600px;
  padding-top: 10em;
  background: #f5f6f3;
}
.resultCard {
  background: white;
  border-radius: 10px;
  margin: 1em;
  padding: 1em;
}
#features {
  padding-top: 5em;
  text-align: center;
}

#features .brand {
  font-size: 1.3em;
}

#features img {
  width: 40px;
}

 .primeButt {
   background: #ff8552;

   border: none;
   border-radius: 0;
 }
</style>
<script>
// @ is an alias to /src
import axios from "axios";

export default {
  name: "home",
  components: {},
  data: function() {
    return {
      results: [],
      query:
        "Is+chronic+breathlessness+lessrecognised+and+treated+compared+with+chronic+pain"
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
    }
  }
};
</script>
