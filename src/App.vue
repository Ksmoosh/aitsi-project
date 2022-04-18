<template>
<div>
  <nav class="header" style="font-family: Arial;">
        <div class="row">
          <div class="col-lg-2 navbar-header-app">
                Lista książek
          </div>
          <div class="col-lg-8 header-sort">
            <div class="row">
                  <div class="col-lg-4">
                    <div>
                        Sortuj po: <b-form-select class="select-style" v-model="selected_sort_option" :options="sort_options"></b-form-select>
                    </div>
                  </div>
                  <div class="col-lg-6">
                    <div>
                        Jak: <b-form-select class="select-style" v-model="selected_sort_type" :options="sort_types"></b-form-select>
                    </div>
                  </div>
                  <div class="col-lg-2 set-sort button-hover" @click="sortBooksClicked">
                    Sortuj
                  </div>
            </div>
            <div class="row">
                  <div class="col-lg-4">
                    Filtruj po: <b-form-select class="select-style" v-model="selected_filter_option" :options="filter_options"></b-form-select>
                  </div>
                  <div class="col-lg-6">
                    <form v-on:submit.prevent="addBook">
                      <div class="row">
                        <div class="col-lg-4">
                          Od: <input v-model="filter_lower" class="input-filter">
                        </div>
                        <div class="col-lg-4">
                          Do: <input v-model="filter_upper" class="input-filter">
                        </div>
                      </div>
                    </form>
                  </div>
                  <div class="col-lg-2 set-filter button-hover" @click="filterBooksClicked">
                    Filtruj
                  </div>
            </div>
          </div>
          <div class="col-lg-2 add-book button-hover" @click="toggleBookCreateModal">
            <p>Dodaj książkę</p>
          </div>
        </div>
    </nav>
  <div id="app">
    <Book ref="book" :filter="filter" :sort="sort"/>
  </div>
  <div v-if="showBookCreateModal">
    <BookAddModal @close="toggleBookCreateModal" @added="addBook"/>
  </div>
  </div>
</template>

<script>
import Book from './components/Book.vue'
import BookAddModal from './components/BookAddModal.vue'

export default {
  name: 'App',
  components: {
    Book, BookAddModal
  },
  data: function () {
    return {
      showBookCreateModal: false,
      selected_sort_option: null,
      selected_sort_type: null,
      selected_filter_option: null,
      filter_upper: null,
      filter_lower: null,
      sort_options: [
          { value: null, text: '-' },
          { value: 'name', text: 'Tytuł' },
          { value: 'author', text: 'Autor' },
          { value: 'genre', text: 'Gatunek' },
          { value: 'rating', text: 'Ocena'},
          { value: 'price', text: 'Cena'}
        ],
      filter_options: [
          { value: null, text: '-' },
          { value: 'price', text: 'Cena' },
        ],
      sort_types: [
        { value: null, text: '-'},
        { value: 'asc', text: 'Rosnąco'},
        { value: 'desc', text: 'Malejąco'},
      ],
      sort: { sort_by: null, type: null },
      filter: { filter_by: null, lower: null, upper: null }
    };
  },
  methods: {
    toggleBookCreateModal: function () {
      this.showBookCreateModal = !this.showBookCreateModal;
    },
    sortBooksClicked: function () {
      this.sort = { sort_by: this.selected_sort_option, type: this.selected_sort_type };
    },
    filterBooksClicked: function () {
      this.filter = { filter_by: this.selected_filter_option, lower: this.filter_lower, upper: this.filter_upper };
    },
    addBook: function (book) {
      this.$refs.book.addBook(book);
      this.showBookCreateModal = !this.showBookCreateModal;
    },
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.header {
  background-color: #ff6700;
  position: fixed;
  z-index: 2;
  height: 80px;
  top: 0;
  left: 0;
  right: 0;
}

.add-book {
  line-height: 80px;
  text-align: center;
  border-left: 2px solid #ff7d26;
  bottom: 0;
  right: 0;
  height: 80px;
  cursor: pointer;
}
.set-sort {
  line-height: 40px;
  text-align: center;
  border-left: 2px solid #ff7d26;
  border-bottom: 1px solid #ff7d26;
  top: 0;
  right: 0;
  height: 40px;
  cursor: pointer;
}
.set-filter {
  line-height: 40px;
  text-align: center;
  border-left: 2px solid #ff7d26;
  bottom: 0;
  right: 0;
  height: 40px;
  cursor: pointer;
}
.header-sort {
  border: 0px;
  line-height: 40px;
  text-align: left;
  height: 80px;
  bottom: 0;
  top: 0;
}
.select-style {
  border-radius: 5px;
  margin-top: 0;
  border: 0;
  background: #ff802b;
  outline: none;
}
.input-filter {
  border: 0px;
  line-height: 20px;
  text-align: center;
  background: #ff802b;
  outline: none;
  height: 20px;
  width: 80px;
  bottom: 0;
  top: 0;
}
.button-hover:hover {
  background-color: #ff7d26;
}

.navbar-header-app {
  font-size: 30px;
  line-height: 80px;
  text-align: center;
  color: #ffffff;
  height: 80px;
  bottom: 0;
  height: 80px;
}
</style>


