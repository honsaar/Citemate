<template>
  <div id="app">

      <b-navbar id="nav" toggleable="lg">
      <!-- logo -->
      <b-navbar-brand class="brand">Citemate.</b-navbar-brand>
      <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>
       <b-collapse id="nav-collapse" is-nav>
      <!-- nav links, pull right -->
       <b-navbar-nav class="ml-auto">
        <b-nav-item> <router-link to="/">Home </router-link> </b-nav-item>
        <b-nav-item> <router-link to="/about">About </router-link></b-nav-item>
        <b-nav-item> <router-link to="/citations">My Citations </router-link></b-nav-item>

        <!-- <b-nav-item> <router-link to="/about"><b-btn>Get Started</b-btn></router-link></b-nav-item> -->
       </b-navbar-nav>
       </b-collapse>
      </b-navbar>

    <router-view/>
  </div>
</template>

<style>
@import url('https://rsms.me/inter/inter.css');
@font-face {
    font-family: Bagnard;
    src: url("./assets/Bagnard.otf") format("opentype");
}

html {

}

#app {
  font-family: 'Inter', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #273043;
  background: #F5F6F3;
}

@supports (font-variation-settings: normal) {
  #app { font-family: 'Inter var', sans-serif; }
}

#nav {
  padding: 30px;
}

#nav a {
  color: #273043;
  padding-right: 1em;
}

#nav a.router-link-exact-active {
  color: #ff8552;
}

.right {
  float: right;
}

.left {
    float: left;
}

.brand {
  font-family: 'Bagnard', serif;
}

.navbar-toggler {
  /* override Bootstrap styling of toggler */
  border: none !important; 
}
</style>
