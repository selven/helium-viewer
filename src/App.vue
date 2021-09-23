<template>
  <div id="app">
    <router-view />
  </div>
</template>

<style>
body {
  padding: 0;
  margin: 0;
  background: rgb(25, 28, 32);
  background: linear-gradient(
    90deg,
    rgba(25, 28, 32, 1) 0%,
    rgba(108, 38, 94, 1) 100%
  );
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>
