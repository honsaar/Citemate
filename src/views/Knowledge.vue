<template>
<div id="section">
    <div class="hero">
      <div class="container">
        <div id="knowledge">
          <h1 class="brand">Citemate Knowledge Centre</h1>
          <p class="subtitle">Be better equipped for your work.</p>
          <br>
          <router-link to="/cite"><b-btn variant="primary" class="primeButt">Create a Bibliography with Citemate</b-btn></router-link>
          <br />
        </div>
      </div>
    </div>
   
    <div class="content" style="background: white;">
        <div class="container">
            <div id="academicReading" style="margin-bottom: 3em;">
                <h2 class="brand">Academic Reading and Research</h2>
                <p>Preamble for academic reading resources</p>
                <p>How to find a useful resource</p>
                <p>What are primary and secondary sources?</p>
                <p>How to effectively read an academic paper</p>
                <p>Why is it frowned upon to use Wikipedia?</p>
            </div>

            <div id="referencing" style="margin-bottom: 5em">
                <h2 class="brand">Referencing</h2>
                <p>General referencing guidelines</p>
                <p><em>Then add in a list of buttons or accordions for common referencing styles</em></p>
            </div>

            <div id="coding" style="margin-bottom: 5em">
                <h2 class="brand">Coding Guidelines</h2>
                <p>Confused about how to write your code for your assignment? Can't tell your Horstmann from your Allman? Look no further, we've got your code covered.</p>
                <p><em>List, with examples, different bracket/indentation styles.</em></p>
            </div>

             <div class="hints">
          <span class="brand">
            <p>I have to reference using a style that Citemate doesn't have!</p></span>
            <p>We try to have everything you might need in your work or studies, but sometimes we can't capture everything. Get in touch with us <a href="mailto:honsyb@gmail.com">via email</a> or tweet to <a href="//twitter.com/honsaar">@honsaar</a> if there is critical information missing or a style that Citemate can't help you with</p>
        </div>
           
        </div>
    </div>
</div> 
</template>

<script>
export default {

}
</script>

<style>
.hints a {
  color: inherit;
  text-decoration: underline;
}

.hints a:hover {
  color: #ff8552;
}
</style>