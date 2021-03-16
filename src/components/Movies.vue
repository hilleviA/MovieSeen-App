<template>
    <section id="movieList">
        <h2>Filmer</h2>
        <!-- Loopar ut movies -->
        <div v-bind:key="movie._id" v-for="movie in movies">

            <div class="movieBox" v-if="editMovie !== movie._id">
                <h3>{{movie.title}}</h3>
                <p class="bold">{{movie.genre}} || {{movie.releaseYear}}</p>   
                <p class="rating">Betyg: {{movie.raiting}}</p>
                <p>Datum då filmen sågs: {{movie.viewDate}} </p>
                <div class="buttons">
                    <button @click="$emit('del-movie', movie._id)" class="deleteButton">Radera</button>
                    <button v-on:click="changeToEdit(movie._id)" class="button">Ändra</button>
                </div>
            </div>
            <div v-else id="updateMovieInput">
                    <input type="text" v-model="movie.title" name="title" /><br/>
                    <input type="text" v-model="movie.genre" name="genre" /><br/>
                    <input type="text" v-model="movie.releaseYear" name="releaseYear" /><br/>
                    <input type="text" v-model="movie.viewDate" name="viewDate" /><br/>
                    <input type="text" v-model="movie.raiting" name="raiting" /><br/>
                    <button v-on:click="updateMovie(movie)">Spara</button>  
            </div>
        </div>
    </section>
</template>

<script>

export default {
    name: "Movies",
    props: ["movies"],
    data() {  
        return {
            editMovie: "" 
        }         
    },
    methods: {
        changeToEdit(id) {
            this.editMovie = id;
        },
        //Uppdatera film
        updateMovie(movie){

            //fetch("http://localhost:3000/movies/update/" + movie._id, {
            fetch("https://moviewatchlistapp.herokuapp.com/movies/update/" + movie._id, {
            method: "PUT",
            headers: { 'Accept': 'application/json', 'Content-Type': 'application/json' },
            body: JSON.stringify(movie),
            })
            .then(() => {
                console.log("Filmen är uppdaterad");         
            })
            .catch(error=> {
                console.log(`Fel: ${error}`);
            })
            //Ändrar tillbaka editiMovie
            this.editMovie = "";
        }
    } 
}
</script>

<style scoped>
 h2 {
     color: #FFF;
     text-align: center;
     padding: 20px;
 }

h3 {
    font-size: 1.2em;
    margin-bottom: 10px;
}
p {
    font-size: 0.8em;
    margin-bottom: 8px;
}

#movieList {
    background-color: #0E996D; 
    padding-bottom: 20px;
}
.movieBox {
    width: 90%;
    border: 1px solid grey;
    padding: 10px;
    margin: 0 auto 10px; 
    background-color: #FFF;
}

.bold {
    font-weight: 600;
}
.rating {
    font-size: 1.2em;
}

.buttons {
    text-align: right;
}

.deleteButton {
    background-color: #DB2819;
    color: #FFF;
    margin-right: 10px;
}
#updateMovieInput {

    background-color: #FFF;
    text-align: center;
    padding: 20px;
}
input[type=text] {
    border: 1px solid grey;
    border-radius: 5px;
    padding: 10px;
    margin: 0 auto 5px;
    width: 90%;
}
</style>