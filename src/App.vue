<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<style>
/* Estilo para a barra de rolagem */
::-webkit-scrollbar {
  width: 8px; /* Largura da barra de rolagem */
  background-color: #000000; /* Cor de fundo da barra de rolagem */
}

/* Estilo para o thumb (alça) da barra de rolagem */
::-webkit-scrollbar-thumb {
  background-color: #1DC46B; /* Cor de fundo do thumb */
  border-radius: 4px; /* Raio de borda do thumb */
}

/* Estilo para o thumb quando estiver sendo arrastado ou em hover */
::-webkit-scrollbar-thumb:hover {
  background-color: #00A556; /* Cor de fundo do thumb em hover */
}

/* Estilo para o thumb quando estiver pressionado */
::-webkit-scrollbar-thumb:active {
  background-color: #00984D; /* Cor de fundo do thumb em pressionado */
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}

body {
    background: rgb(49,71,94);
    background: linear-gradient(90deg, rgba(49,71,94,1) 0%, rgba(49,71,94,1) 35%, rgba(29,196,107,1) 100%);
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
