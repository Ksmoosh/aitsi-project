<template>
    <div class="row">
        <div class="backdrop" @click.self="closeEditBookModal">
            <div class="col-lg-4 mx-auto">
                <div class="modal-book-details">
                    <div class="card">
                        <div class="card-body" style="text-align: left;">
                            <div class="row">
                                <form v-on:submit.prevent="editBook">
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
    props: ['book'],
    data() {
        return {
            name: this.book.name,
            author: this.book.author,
            genre: this.book.genre,
            rating: this.book.rating,
            price: this.book.price,
            description: this.book.description,
            image: this.book.image,
        }
    },
    methods: {
        closeEditBookModal() {
            this.$emit('closeEdit')
        },
        editBook(){
            var book = {
                name: this.name,
                author: this.author,
                genre: this.genre,
                rating: this.rating,
                price: this.price,
                description: this.description,
                image: this.image,
            }
            this.$emit('edited', book)
        }
    }
};
</script>
