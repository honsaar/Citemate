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
         <router-link to="/cite"><b-btn variant="primary" class="primeButt">Create a Bibliography</b-btn></router-link>
      </div>
    </div>

    <div class="content" style="background: white;">
      <div class="container">
        <div id="features" style="margin-bottom: 2em;">
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
     
        <div id="showcase">
          <!-- Insert screenshots of a Citation list in the browser. Like an app showcase -->
          <br><br><br>
        </div>

        <div id="guides">
          <div class="hints">
          <span class="brand">
            <p><strong>Citation Guides</strong></p></span>
            <p>Citemate can help compile your bibliography, but referencing processes and academic reading guides are freely available for you on <router-link to="/knowledge">Citemate's knowledge centre</router-link>.</p>
        </div>
        </div>

        <div id="testimonialsSupport">
          <!-- insert testimonials/social proofs, as well as any support-related information. Who to contact if there are issues, and ways you can support the application -->
        </div>

      </div>
    </div>
  </div>
</template>

<style scoped>
a {
  color: #ff8552;
}
</style>

