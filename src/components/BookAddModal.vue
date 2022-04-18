<template>
    <div class="row">
        <div class="backdrop" @click.self="closeAddBookModal">
            <div class="col-lg-4 mx-auto">
                <div class="modal-book-details">
                    <div class="card">
                        <div class="card-body" style="text-align: left;">
                            <div class="row">
                                <form v-on:submit.prevent="addBook">
                                    Tytuł:<br>
                                    <div class="row">
                                        <input v-model="name">
                                    </div>
                                    Autor:<br>
                                    <div class="row">
                                        <input v-model="author">
                                    </div>
                                    Gatunek:<br>
                                    <div class="row">
                                        <input v-model="genre">
                                    </div>
                                    Ocena:<br>
                                    <div class="row">
                                        <input v-model="rating">
                                    </div>
                                    Cena:<br>
                                    <div class="row">
                                        <input v-model="price">
                                    </div>
                                    Link do zdjęcia:<br>
                                    <div class="row">
                                        <input v-model="image">
                                    </div>
                                    Opis:<br>
                                    <div class="row">
                                        <input v-model="description">
                                    </div><br>
                                    <div class="row">
                                        <button type="submit">Zatwierdź</button>
                                    </div>
                                </form>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'BookEditModal',
    data: function () {
        return {
            name: '',
            author: '',
            genre: '',
            rating: '',
            price: '',
            description: '',
            image: ''
        };
    },
    methods: {
        closeAddBookModal() {
            this.$emit('close')
        },
        addBook(){
            var book = {
                name: this.name,
                author: this.author,
                genre: this.genre,
                rating: this.rating,
                price: this.price,
                description: this.description,
                image: this.image,
            }
            this.$emit('added', book)
        }
    }
};
</script>

<style scoped>
    .backdrop {
        top: 0;
        z-index: 3;
        position: fixed;
        background: rgba(0,0,0,0.5);
        width: 100%;
        height: 100%;
        top: 0;
        left: 0;
        right: 0;
    }
</style>
