<template>
  <div id="nav">
    <router-link to="/">Domov</router-link> |
    <router-link to="/spravovat">Spravovať</router-link>
  </div>
  <router-view />
</template>

<style lang="scss">
* {
  margin: 0;
  padding: 0;
}
#app {
  font-family: 'Poppins', sans-serif; //300 - light, 400 - regular, 700 - bold
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
input,
textarea {
  font-family: 'Poppins', sans-serif;
}

#nav {
  padding: 30px;
  background-color: #efefef;

  a {
    font-weight: bold;
    color: #2e2e2e;
    text-decoration: none;

    &.router-link-exact-active {
      color: #05457e;
      text-decoration: underline;
    }
  }
}
</style>
