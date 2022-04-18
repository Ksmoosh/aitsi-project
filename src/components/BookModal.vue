<template>
    <div class="row">
        <div class="backdrop" @click.self="closeBookModal">
            <div class="col-lg-4 mx-auto">
                <div class="modal-book-details">
                    <div class="card">
                        <img class="card-img-top mx-auto" :src="book.image"  :alt="book.name">
                        <div class="card-body" style="text-align: left;">
                            <div class="row">
                                Tytuł: {{ book.name }}<br>
                                Autor: {{ book.author }}<br>
                                Gatunek: {{ book.genre }}<br>
                                Ocena: {{ book.rating }}<br>
                                Cena: {{ book.price }}<br>
                                Opis:<br>{{ book.description }}
                            </div>
                            <div class="row">
                                <button @click="editBookModal">Edytuj</button>
                                <button @click="deleteBook">Usuń</button>
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
    name: 'BookModal',
    props: ['book'],
    methods: {
        closeBookModal() {
            this.$emit('close')
        },
        editBookModal() {
            this.$emit('edit')
        },
        deleteBook() {
            this.$emit('delete')
        },
    }
};
</script>


<style>
    .modal-book-details {
        padding: 20px;
        margin: 20px auto;
        background: white;
        border-radius: 10px;
        overflow-y: auto;
        max-height: 90vh;
    }
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

