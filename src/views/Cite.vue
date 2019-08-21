<template>
  <div class="about">
    <div class="hero">
      <div class="container">

        <!-- use this layout if the bibliography is empty, otherwise change the wording and layout of the top -->

        <div id="refStyle" v-if="selected == ''">
          <h1 class="brand">First, choose your reference style</h1>
          <p class="subtitle">Don't worry, you can change your reference style at any point.</p>
          <br />
          <b-btn variant="primary" class="secButt" @click="chooseStyle('Harvard')">Harvard</b-btn>
          <b-btn variant="primary" class="secButt" @click="chooseStyle('MLA')">MLA</b-btn>
          <b-btn variant="primary" class="secButt" @click="chooseStyle('Chicago')">Chicago</b-btn>
          <b-btn variant="primary" class="secButt" @click="chooseStyle('Vancouver')">Vancouver</b-btn>
          <br />
          <span class="styleList" @click="showMore = !showMore">The style I want isn't listed</span>
          <br /><br/>
          <div v-if="showMore">
            <p>Choose from a further selection of referencing styles:</p>
             <b-form-select class="refLists" v-model="selected" :options="options" @change="chooseStyle(selected)"></b-form-select>
          </div>
          <br><br>
        </div>

        <!-- Choose your source -->
        <div id="refStyle" v-if="selected != '' && sourceType == ''">
          <h1 class="brand">Next, what are you referencing?</h1>
          <p class="subtitle">Different resource types are referenced slightly differently</p>
          <br />
            <b-row>
              <b-col>
                <div class="sources">
                  <img src="../assets/book.svg" width="30"/>
                  <p>Book</p>
                </div>
              </b-col>
              <b-col>
                <div class="sources" @click="sourceType='journal'">
                  <img src="../assets/diary.svg" width="30"/>
                  <p>Journal Article</p>
                </div>
              </b-col>
              <b-col>
                <div class="sources">
                  <img src="../assets/newspaper.svg" width="30"/>
                  <p>Newspaper Article</p>
                </div>
              </b-col>
              <b-col>
                <div class="sources">
                  <img src="../assets/globe.svg" width="30"/>
                  <p>Website</p>
                </div>
              </b-col>
            </b-row>
            <!-- <b-row>
              <b-col>
                <div class="sources">
                  <p>Book</p>
                </div>
              </b-col>
              <b-col>
                <div class="sources">
                  <p>Book</p>
                </div>
              </b-col>
              <b-col>
                <div class="sources">
                  <p>Book</p>
                </div>
              </b-col>
              <b-col>
                <div class="sources">
                  <p>Book</p>
                </div>
              </b-col>
            </b-row> -->
          <br />
          <br><br>
        </div>


      <!-- search for your resource -->
    <div id="refStyle" v-if="selected != '' && sourceType != ''">
          <h1 class="brand">Finally, search for your resource</h1>
          <!-- change wording based on what type -->
          <p class="subtitle" v-if="sourceType=='journal'">Citemate searches for your journal article using <strong>CrossRef's</strong> data libraries.</p>
          <br />
         <b-form @submit="cite" class="refLists centered">
             <b-form-input v-model="query" placeholder="Journal Search" class="refLists"></b-form-input>
            </b-form>
           
          <br />
        </div>

      </div>
    </div>
    <div class="content" style="background: white;padding-top:5em;">
      <br />
      <br />
      <br />
      <div style="text-align: center;">
         <!-- <b-btn variant="primary" class="primeButt" @click="cite()">Test cite, yo</b-btn> -->
         <div id="emptyState" v-if="bibliography.length < 1 && !searching">
           <img src="../assets/empty.svg" width="300"/>
          <h2 class="brand">Your bibliography is currently empty</h2>
          <p>This is where your reference list will appear once you add an information source</p>
          
          </div>
        </div>
      <br />
      <br />
      <!-- show the results differently. In a modal? -->
      <div class="results container">
        <div class="resultCard" v-for="(paper, key) in results" :key="key">
          <p>
            <strong>{{paper.title}}</strong>
          </p>
          <p>
            <span v-for="(author, index) in paper.authors" :key="index">
            {{author.given}} {{author.family}}<span v-if="index < paper.authors.length-1">,</span>
            </span>
          </p>
          <b-badge pill variant="primary" class="journalPill">{{paper.journal}}</b-badge>
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
      query: "",
      showMore: false,
      options: [
        {value: 'Harvard', text: 'Harvard'},
        {value: 'MLA', text: 'MLA'},
        {value: 'Chicago', text: 'Chicago'},
        {value: 'Vancouver', text: 'Vancouver'},
        {value: 'APA', text: 'APA'},
        {value: 'CSIRO', text: 'CSIRO'},
      ],
      selected: '',
      sourceType: '',
      searching: false
    };
  },
  methods: {
    cite(evt) {
      evt.preventDefault();
      this.results = [];
      this.searching = true;
      console.log("Loading response...");

      //UPDATE THIS so that CrossRef is making better API calls as per their guidelines
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
            //look for authors
            tempResult.authors = search.author;
            //if no authors, look for editors
            tempResult.published.year = search.created["date-parts"][0][0];
            tempResult.published.month = search.created["date-parts"][0][1];
            tempResult.published.day = search.created["date-parts"][0][2];
            tempResult.issue = search.issue;
            tempResult.publisher = search.publisher;
            tempResult.link = search.URL;
            tempResult.volume = search.volume;
            if(search["container-title"] != undefined){
            tempResult.journal = search["container-title"][0];
            }
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
      if(this.selected !== style){
        this.selected = style;
      }
      console.log(this.selected);
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

#emptyState {
  color: #d6d7d4;
}

.refLists {
  max-width: 400px;
  border: none;
  border-radius: 0;
}

.sources {
  vertical-align: middle;
  padding-top: 13%;
  /* width: 120px; */
  height: 120px;
  background: #d6d7d4 !important;
  border: 1px solid #d6d7d4 !important;
  border-radius: 0 !important;
  color: #727270 !important;
  margin: 1em;
  cursor: pointer;
}
.sources:hover {
  background: #c4c6c1 !important;
  border: 1px solid #c4c6c1 !important;
}

.sources:active {
  background: #b4b5b2 !important;
  border: 1px solid #b4b5b2 !important;

}
.centered {
  margin-left: auto;
  margin-right: auto;
}

.resultCard {
  background: #f5f6f3;
  padding: 2em;
  margin: 1em;
}

.journalPill {
  background: #d6d7d4;
  color: #727270;
  margin-top: 1em;
  margin-left: -5px;
}
</style>
