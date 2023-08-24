<template>
    <div class="movie-add-form">
        <h2>Yangi kino qo'shish</h2>
        <form class="add-form d-flex gap-2" @submit.prevent>

            <input class="form-control new-movie-label" type="text" placeholder="Qanday kino?" :value="name"
                @input="name = $event.target.value">

            <input class="form-control new-movie-label" type="number" placeholder="Nechi marotaba ko'rilgan?"
                :value="viewers" @input="viewers = $event.target.value">

            <button class="btn btn-outline-dark" type="submit" @click="addMovie">Qo'shish</button>
        </form>
    </div>
</template>

<script>
export default {
    data() {
        return {
            name: '',
            viewers: '',
        }
    },
    methods: {
        addMovie() {
            if(!this.name || !this.viewers) {
                return alert("Kino qo'shing")
            }
            const newMovie = {
                name: this.name,
                viewers: this.viewers,
                favourite: false,
                like: false,
            }
            this.$emit('createMovie', newMovie)
            this.name = ''
            this.viewers = ''
        }
    },
}
</script>

<style>
.movie-add-form {
    margin-top: 2rem;
    padding: 1.5rem;
    background-color: #fcfaf5;
    border-radius: 4px;
    box-shadow: 15px 15px 15px rgb(0, 0, 0, 0.15);
    list-style: none;
}
</style>