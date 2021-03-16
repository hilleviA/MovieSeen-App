<template>
<section id="newMovie">   
    <h2>Lägg till ny film</h2>
    <form @submit="addMovie">
        <label for="title">Titel</label>
        <input type="text" v-model="title" name="title" id="title" placeholder="Filmens titel">
        <label for="genre">Genre</label>
        <input type="text" v-model="genre" name="genre" id="genre" placeholder="Filmens genre">
        <label for="releaseYear">Release År</label>
        <input type="text" v-model="releaseYear" name="releaseYear" id="releaseYear" placeholder="Året då filmen släpptes">
        <label for="viewDate">Datum då filmen sågs</label>
        <input type="text" v-model="viewDate" name="viewDate" id="viewDate" placeholder="Datum då filmen sågs">
        <label for="raiting">Betyg</label>
        <input type="text" v-model="raiting" name="raiting" id="raiting" placeholder="Betyg">
        <input type="submit" value="Lägg till" class="button">
    </form>
</section>
</template>

<script>
export default {
    name: "AddNewMovie",
    data() {
        return {
            title: '',
            genre: '',
            releaseYear: '',
            viewDate: '',
            raiting: ''
        }
    },
    methods: {
        addMovie(e) {
            e.preventDefault();

            //Skapar nytt objekt och skickar till app.vue
            const newMovie = {
                title: this.title,
                genre: this.genre,
                releaseYear: this.releaseYear,
                viewDate: this.viewDate,
                raiting: this.raiting
            }
            this.$emit("add-movie", newMovie);

            //Tömmer formuläret
            this.title = "";
            this.genre = "";
            this.releaseYear = "";
            this.viewDate = "";
            this.raiting = "";
        }
    }
}
</script>

<style scoped>

#newMovie {
    border: 2px solid #0E996D;
    margin: 20px auto;
}

h2 {
    padding: 20px;
}

form {
    padding: 20px;
    width: 100%;
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;  
}

input[type=text] {
    width: 60%;
    border: 1px solid grey;
    border-radius: 5px;
    padding: 10px;
    margin-bottom: 5px;
}

label {
    width: 35%;
}


</style>