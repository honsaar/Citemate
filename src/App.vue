<template>
  <div id="app">
    <b-navbar id="nav" toggleable="lg">
      <!-- logo -->
      <b-navbar-brand class="brand"><router-link to="/">Citemate<sup class="brandSup">[1]</sup></router-link></b-navbar-brand>
      <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>
      <b-collapse id="nav-collapse" is-nav>
        <!-- nav links, pull right -->
        <b-navbar-nav class="ml-auto">
          <b-nav-item>
            <router-link to="/">Home</router-link>
          </b-nav-item>
          <b-nav-item>
            <router-link to="/knowledge">Knowledge Centre</router-link>
          </b-nav-item>
          <b-nav-item>
            <router-link to="/cite"><b-btn variant="primary" class="secButt" style="margin: auto;margin-top: -7px;">Your Bibliography</b-btn></router-link>
          </b-nav-item>

          <!-- <b-nav-item> <router-link to="/about"><b-btn>Get Started</b-btn></router-link></b-nav-item> -->
        </b-navbar-nav>
      </b-collapse>
    </b-navbar>

    <router-view />
  </div>
</template>

<style>
@import url("https://rsms.me/inter/inter.css");
@font-face {
  font-family: Bagnard;
  src: url("./assets/Bagnard.otf") format("opentype");
}

.brandSup {
  top: -1em;
  font-size: 0.5em;
}

#app {
  font-family: "Inter", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #273043;
}

@supports (font-variation-settings: normal) {
  #app {
    font-family: "Inter var", sans-serif;
  }
}

#nav {
  padding: 30px;
  z-index: 99;
}

#nav a {
  color: #273043;
  padding-right: 1em;
}

#nav a.router-link-exact-active {
  color: #ff8552;
}

.brand a.router-link-exact-active {
  color: #273043 !important;
}

.right {
  float: right;
}

.left {
  float: left;
}

.brand {
  font-family: "Bagnard", serif;
  
}

.navbar-toggler {
  /* override Bootstrap styling of toggler */
  border: none !important;
}
.hero {
  text-align: center;
min-height: 600px;
padding-top: 15em;
background: #f5f6f3;
padding-bottom: 2em;
position: relative;
top: -7em;
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
  background: #ff8552 !important;
  border: 1px solid #ff8552 !important;
  border-radius: 0 !important;
}
.primeButt:hover {
  background: #ce693e !important;
  border: 1px solid #ce693e !important;
}
.primeButt:active {
  /* override bootstrap here again */
  background: #c2633a !important;
  border: 1px solid #c2633a !important;
}

.secButt {
  background: #d6d7d4 !important;
  border: 1px solid #d6d7d4 !important;
  border-radius: 0 !important;
  color: #727270 !important;

}
.secButt:hover {
  background: #c4c6c1 !important;
  border: 1px solid #c4c6c1 !important;
}

.secButt:active {
  background: #b4b5b2 !important;
  border: 1px solid #b4b5b2 !important;

}

.styleChoice .secButt {
  margin: 1em;
}

.hints {
  background:#f5f6f3;
  padding: 2em;
  text-align: center;

}
</style>
