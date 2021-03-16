<template>
  <div id="app">
    <Header />
    <AddNewMovie v-on:add-movie="addMovie" />
    <Movies v-bind:movies="movies" v-on:del-movie="deleteMovie" /> 
  </div>
</template>

<script>
//Importerar components
import Header from './components/layout/Header';
import Movies from './components/Movies';
import AddNewMovie from './components/AddNewMovie';

export default {
  name: 'App',
  components: {
    Movies,
    Header,
    AddNewMovie

  },
  data() {
    return {
      movies: []
    }
  },
  methods: {
    //Tar bort film
    deleteMovie(id) {
      fetch("https://moviewatchlistapp.herokuapp.com/movies/delete/" + id, {
        method: "DELETE",
      })
      .then(() => {
        console.log("Filmen är borttagen")
        //Skapar ny array utan det borttagna objektet     
        this.movies = this.movies.filter(movie => movie._id !== id);   
      })
      .catch(error=> {
          console.log(`Fel: ${error}`);
      }) 
    },
    //Lägger till ny film
    addMovie(newMovie) {
      
      fetch("https://moviewatchlistapp.herokuapp.com/movies/add", {
      method: "POST",
      headers: { 'Accept': 'application/json', 'Content-Type': 'application/json' },
      body: JSON.stringify(newMovie),
      })
     .then(() => {
        console.log("Filmen är tillagd");
        //Lägger till det nya objektet i arrayen
        this.movies = [...this.movies, newMovie];       
      })
      .catch(error=> {
          console.log(`Fel: ${error}`);
      }) 
    }
  },
  //Läser in filmer
  created() {
    fetch("https://moviewatchlistapp.herokuapp.com/movies")
    .then(response => response.json())
    .then((data) => {
      this.movies = data;
    })  
    .catch(error=> {
          console.log(`Fel: ${error}`);
      });
  }
}
</script>

<style>

* {
  box-sizing: border-box; 
  margin: 0;
  padding: 0;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  width: 100%;
  color: #2c3e50;
}
section {
  width: 100%;
  max-width: 800px;
  margin: 0 auto;
}

h2 {
  font-size: 1.8em;
}

p {
  font-size: 0.9em;
}

button {
  border-radius: 8px;
  padding: 8px;
  border: none;
  background-color: #5F8A64;
  color: #FFF;
}

input[type=submit] {
  border-radius: 8px;
  padding: 8px;
  border: none;
  background-color: #5F8A64;
  color: #FFF;
}
</style>
