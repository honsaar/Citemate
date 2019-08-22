<template>
  <div class="about">
    <div class="hero" :class="addingSource || changingStyle ? '' : 'hideHero'">
      <div class="container">
        <!-- use this layout if the bibliography is empty, otherwise change the wording and layout of the top -->
      <div>
        <div id="refStyle" v-if="selected == '' && addingSource || selected == '' && changingStyle">
          <h1 class="brand" v-if="changingStyle">Which reference style do you need?</h1>
          <h1 class="brand" v-else>First, choose your reference style</h1>
          <p class="subtitle">Don't worry, you can change your reference style at any point.</p>
          <br />
          <div class="styleChoice">
            <b-btn variant="primary" class="secButt" @click="chooseStyle('Harvard')">Harvard</b-btn>
            <b-btn variant="primary" class="secButt" @click="chooseStyle('MLA')">MLA</b-btn>
            <b-btn variant="primary" class="secButt" @click="chooseStyle('Chicago')">Chicago</b-btn>
            <b-btn variant="primary" class="secButt" @click="chooseStyle('Vancouver')">Vancouver</b-btn>
          </div>
          <br />
          <span class="styleList" @click="showMore = !showMore">The style I want isn't listed</span>
          <br />
          <br />
          <div v-if="showMore">
            <p>Choose from a further selection of referencing styles:</p>
            <b-form-select
              class="refLists"
              v-model="selected"
              :options="options"
              @change="chooseStyle(selected)"
            ></b-form-select>
          </div>
          <br />
          <br />
        </div>
        </div>

        <!-- Choose your source -->
        <div id="refStyle" v-if="selected != '' && sourceType == '' && addingSource">
          <h1 class="brand" v-if="bibliography.length < 1">Next, what are you referencing?</h1>
           <h1 class="brand" v-else>What are you referencing?</h1>
          <p class="subtitle">Different resource types are referenced slightly differently</p>
          <br />
          <b-row>
            <b-col sm="12" md="6" lg="3">
              <div class="sources">
                <span class="sourceText">
                  <img src="../assets/book.svg" width="30" />
                  <p>Book</p>
                </span>
              </div>
            </b-col>
            <b-col sm="12" md="6" lg="3">
              <div class="sources" @click="sourceType='journal'">
                <span class="sourceText">
                  <img src="../assets/diary.svg" width="30" />
                  <p>Journal Article</p>
                </span>
              </div>
            </b-col>
            <b-col sm="12" md="6" lg="3">
              <div class="sources">
                <span class="sourceText">
                  <img src="../assets/newspaper.svg" width="30" />
                  <p>Newspaper Article</p>
                </span>
              </div>
            </b-col>
            <b-col sm="12" md="6" lg="3">
              <div class="sources">
                <span class="sourceText">
                  <img src="../assets/globe.svg" width="30" />
                  <p>Website</p>
                </span>
              </div>
            </b-col>
          </b-row>
        </div>

        <!-- search for your resource -->
        <div id="refStyle" v-if="selected != '' && sourceType != '' && addingSource">
          <h1 class="brand">Finally, search for your resource</h1>
          <!-- change wording based on what type -->
          <p class="subtitle" v-if="sourceType=='journal'">
            Citemate searches for your journal article using
            <strong>CrossRef's</strong> data libraries.
          </p>
          <br />
          <b-form @submit="cite" class="refLists centered">
            <b-form-input v-model="query" placeholder="Journal Search" class="refLists"></b-form-input>
            <b-button
              type="submit"
              variant="primary"
              class="primeButt"
              style="margin-top:-3px;"
            >Search</b-button>
          </b-form>

        </div>
      </div>
    </div>
    <div class="content" style="background: white;padding-top:0em;">


 <!-- show paper details -->
      <div class="results container" style="margin-left: auto;margin-right: auto;">
        <div class="resultCard" v-if="adding">
          <b-row>
            <b-col>
              <h3 class="brand" style="margin-bottom: 1em;">Here's what we could find for this source:</h3>
              <div>
              <h4>
                {{adding.title}}
              </h4>
              <p>
                <strong>Authors:</strong>
                <span v-for="(author, index) in adding.authors" :key="index">
                  {{author.given}} {{author.family}}
                  <span v-if="index < adding.authors.length-1">,</span>
                </span>
              </p>
              <p>
                <strong>Journal:</strong>
                {{adding.journal}}
              </p>
              <p>
                <strong>Volume:</strong>
                {{adding.volume}}
              </p>
              <p>
                <strong>Issue:</strong>
                {{adding.issue}}
              </p>
              <p>
                <strong>Pages:</strong>
                {{adding.pages}}
              </p>
              <p>
                <strong>Publisher:</strong>
                {{adding.publisher}}
              </p>
              <p>
                <strong>Date:</strong>
                {{adding.published.day}}-{{adding.published.month}}-{{adding.published.year}}
              </p>
               <p style="text-align: center">
                <b-btn variant="primary" class="secButt" @click="addToList(adding)">That looks right</b-btn>
              </p>
              </div>
            </b-col>
          </b-row>
        </div>
      </div>

      <!-- emptyState -->
        <div id="emptyState" v-if="bibliography.length < 1 && !searching && !addingSource && !adding" style="text-align: center;">
          <img src="../assets/empty.svg" width="300" />
          <h2 class="brand">Your bibliography is empty</h2>
          <p>This is where your reference list will appear once you add an information source</p>
           <p style="text-align: center">
                <b-btn variant="primary" class="primeButt" @click="addingSource = true">Add your first reference</b-btn>
              </p>
        </div>


        <!-- if bibliography isn't empty -->
        <div class="results container" v-if="!searching && !adding && !addingSource && bibliography.length > 0">
          <h1 class="brand">Your references</h1>
          <p class="subtitle">Your bibliography currently has {{bibliography.length}} item<span v-if="bibliography.length > 1">s</span>.</p>
          <p v-if="!changingStyle">You are currently using <strong>{{selected}}</strong> referencing style. &nbsp;&nbsp;<b-button variant="primary" class="primeButt" @click="changeStyle()">Change</b-button></p>

          

        <div class="resultCard" v-for="(paper, key) in bibliography" :key="key">

              <p>
                <strong>{{paper.title}}</strong>
              </p>
              <p>
                <span v-for="(author, index) in paper.authors" :key="index">
                  {{author.given}} {{author.family}}
                  <span v-if="index < paper.authors.length-1">,</span>
                </span>
              </p>
              <p class="journalPill">{{paper.journal}}</p>

        </div>
        <b-button variant="primary" class="primeButt" @click="addingSource = true">Add a reference</b-button>
      </div>


      <div v-if="loading">
        <div class="gooey">
          <span class="dot"></span>
          <div class="dots">
            <span></span>
            <span></span>
            <span></span>
          </div>
        </div>
      </div>

      <!-- show the results -->
      <div class="results container">
        <h2 class="brand" v-if="results.length > 0">Journal results</h2>
        <div class="resultCard" v-for="(paper, key) in results" :key="key">
          
          <b-row>
            <b-col sm="12" md="10">
              <p>
                <strong>{{paper.title}}</strong>
              </p>
              <p>
                <span v-for="(author, index) in paper.authors" :key="index">
                  {{author.given}} {{author.family}}
                  <span v-if="index < paper.authors.length-1">,</span>
                </span>
              </p>
              <p class="journalPill">{{paper.journal}}</p>
            </b-col>
            <b-col sm="12" md="2">
              <p style="text-align: center">
                <b-btn variant="primary" class="secButt resultButt" @click="assessSource(paper)">Cite</b-btn>
              </p>
            </b-col>
          </b-row>
        </div>
      </div>

     
    </div>
  </div>
</template>

<script>
//FIXME: (not a fix, just so there's a highlight in Code) use this list for guides and a list of different styles: http://www.citethisforme.com/guides
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
        { value: "Harvard", text: "Harvard" },
        { value: "MLA", text: "MLA" },
        { value: "Chicago", text: "Chicago" },
        { value: "Vancouver", text: "Vancouver" },
        { value: "APA", text: "APA" },
        { value: "CSIRO", text: "CSIRO" }
      ],
      selected: "",
      sourceType: "",
      searching: false,
      loading: false,
      adding: undefined,
      addingSource: false,
      changingStyle: false
    };
  },
  methods: {
    cite(evt) {
      evt.preventDefault();
      this.results = [];
      this.searching = true;
      console.log("Loading response...");
      this.loading = true;
      //add &mailto=honsyb@gmail.com to the query request once testing done
      axios
        // .get("https://api.crossref.org/works?query=" + this.query + "&rows=10")
        .get("https://api.crossref.org/works?sample=10")
        .then(response => {
          console.log("Loaded!");
          this.loading = false;
          for (let i = 0; i < response.data.message.items.length; i++) {
            var search = response.data.message.items[i];
            if (search.type == "journal-article") {
              //only return journal articles
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
              if (search["container-title"] != undefined) {
                tempResult.journal = search["container-title"][0];
              }
              tempResult.pages = search.page;
              this.results.push(tempResult);
              this.addingSource = false;
            }
          }
        });
    },
    book() {
      console.log("Loading response for book libraries...");
    },
    styleList() {
      console.log("Loading list of other styles...");
    },
    changeStyle(){
      this.selected = '';
      // this.addingSource = true;
      this.changingStyle = true;
    },
    chooseStyle(style) {
      console.log("Current reference style is " + style);
      if (this.selected !== style) {
        this.selected = style;
      }
      console.log(this.selected);
      this.changingStyle = false;
    },
    assessSource(source) {
      //remove search results, show paper details and confirm if it needs to be added.
      this.results = [];
      this.searching = false;
      this.adding = source;
      console.log(this.adding);
      this.addingSource = false;
      //also hide the top area, as that's only required when adding a source
      //create new top bar area to prompt users to add a new source
    },
  addToList(source){
    console.log("adding to list");
    this.bibliography.push(source);
    this.adding = undefined;
    this.addingSource = false;
    this.sourceType = '';
  }
  }
};
</script>

<style>
.hideHero {
  height: 0px !important;
  min-height: 0px;
  padding: 0;
  transition: all 0.7s ease-in-out;
}
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
  margin-top: 20%;
}

.refLists {
  max-width: 600px;
  border: none;
  border-radius: 0;
  display: inline;
  margin-bottom: 1em;
}

.refLists .form-control:focus {
  box-shadow: none;
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
  position: relative;
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
  padding: 1.5em;
  margin: 1em;
  font-size: 80%;
  line-height: 1.2;
}

.resultCard p {
  margin-bottom: 0.5rem;
}

.journalPill {
  /* background: #d6d7d4; */
  color: #727270;
  margin-top: 1em;
  font-size: 0.8em;
  font-weight: 700;
}
.sourceText {
  position: absolute;
  top: 50%;
  left: 25%;
  width: 100%;
  margin: -15% 0 0 -25%;
}

@media only screen and (max-width: 990px) {
  .sourceText {
    top: 65%;
  }
}
@media only screen and (max-width: 768px) {
  .sourceText {
    top: 75%;
  }
  .sources {
    margin-bottom: 0em;
  }
  .gooey {
    /* override margin on smaller screens */
    margin-top: 20px !important;
  }
}

.resultButt {
  width: 100%;
  margin-top: 1em;
}

.gooey {
  position: absolute;
  top: 47%;
  left: 50%;
  width: 142px;
  height: 40px;
  margin: -20px 0 0 -71px;
  background: #fff;
  filter: contrast(20);
}
.gooey .dot {
  position: absolute;
  width: 16px;
  height: 16px;
  top: 12px;
  left: 15px;
  filter: blur(4px);
  background: #000;
  border-radius: 50%;
  transform: translateX(0);
  animation: dot 2.8s infinite;
}
.gooey .dots {
  transform: translateX(0);
  margin-top: 12px;
  margin-left: 31px;
  animation: dots 2.8s infinite;
}
.gooey .dots span {
  display: block;
  float: left;
  width: 16px;
  height: 16px;
  margin-left: 16px;
  filter: blur(4px);
  background: #000;
  border-radius: 50%;
}
@-moz-keyframes dot {
  50% {
    transform: translateX(96px);
  }
}
@-webkit-keyframes dot {
  50% {
    transform: translateX(96px);
  }
}
@-o-keyframes dot {
  50% {
    transform: translateX(96px);
  }
}
@keyframes dot {
  50% {
    transform: translateX(96px);
  }
}
@-moz-keyframes dots {
  50% {
    transform: translateX(-31px);
  }
}
@-webkit-keyframes dots {
  50% {
    transform: translateX(-31px);
  }
}
@-o-keyframes dots {
  50% {
    transform: translateX(-31px);
  }
}
@keyframes dots {
  50% {
    transform: translateX(-31px);
  }
}
</style>
