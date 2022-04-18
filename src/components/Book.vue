<template>
<div>
  <div class="grid">
    <div class="books-list">
      <div v-for="book in filteredBooks" v-bind:key="book">
        <div class="row g-0 p-2 justify-content-center">
          <div class="col-lg-3 justify-content-center">
            <div class="card" @click="toggleBookModal(book)">
                <img class="card-img-top mx-auto" :src="book.image"  :alt="book.name">
              <div class="card-body" style="text-align: left;">
                <div class="row">
                    Tytuł: {{ book.name }}<br>
                    Autor: {{ book.author }}<br>
                    Gatunek: {{ book.genre }}<br>
                    Ocena: {{ book.rating }}<br>
                    Cena: {{ book.price }}
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>   
    </div>         
  </div>
  <div v-if="showBookModal">
    <BookModal :book="clickedBook" @close="toggleBookModal" @edit="editBook" @delete="deleteBook"/>
  </div>
  <div v-if="showBookEditModal">
    <BookEditModal :book="clickedBook" @closeEdit="closeEditBook" @edited="updateBook"/>
  </div>
</div>
</template>

<script>
import BookModal from './BookModal.vue'
import BookEditModal from './BookEditModal.vue'
export default {
  name: "HelloWorld",
  components: { BookModal, BookEditModal },
  props: ['filter', 'sort'],
  computed: {
    filteredBooks: function () {
      let modified_books = this.books;
      if (this.filter.filter_by != null && parseFloat(this.filter.upper) >= 0 && parseFloat(this.filter.lower) >= 0) {
        modified_books = modified_books.filter(this.checkFilter);
      }
      if (this.sort.sort_by !== null && this.sort.type !== null){
        modified_books.sort(this.sortFunction(this.sort));
      }
      return modified_books
    }
  },
  data: function () {
    return {
      books: [],
      showBookModal: false,
      showBookEditModal: false,
      clickedBook: {name: "", author: "", image: "", price: "", rating: "", genre: "", description: ""},
    };
  },
  methods: {
    sortFunction: function(sort) {
      return function(a, b) {
          var valA;
          var valB;
          if (["name", "author", "genre"].indexOf(sort.sort_by) > -1) {
            valA = a[sort.sort_by].toUpperCase(); // ignore upper and lowercase
            valB = b[sort.sort_by].toUpperCase();
          } else if (["rating", "price",].indexOf(sort.sort_by) > -1) {
            valA = parseFloat(a[sort.sort_by]);
            valB = parseFloat(b[sort.sort_by]);
          } else {
            return 0;
          }
          let multiplier = 1;
          if (sort.type == "asc") {
            multiplier *= -1;
          }
          if (valA < valB) {
            return 1 * multiplier;
          }
          if (valA > valB) {
            return -1 * multiplier;
          }

          // values must be equal
          return 0;
        }
    },
    checkFilter: function(book) {
      return parseFloat(book[this.filter.filter_by]) >= parseFloat(this.filter.lower) && parseFloat(book[this.filter.filter_by]) <= parseFloat(this.filter.upper);
    },
    addBook: function (book) {
        this.books.push({name: book.name, author: book.author, image: book.image, price: book.price, rating: book.rating, genre: book.genre, description: book.description})
        localStorage.setItem('books', JSON.stringify(this.books));
    },
    toggleBookModal: function (book = {name: "", author: "", image: "", price: "", rating: "", genre: "", description: ""}) {
      this.clickedBook = book;
      this.showBookModal = !this.showBookModal;
    },
    editBook: function () {
      this.showBookModal = !this.showBookModal;
      this.showBookEditModal = !this.showBookEditModal;
    },
    deleteBook: function () {
      this.books = this.books.filter(x => x !== this.clickedBook);
      localStorage.setItem('books', JSON.stringify(this.books));
      this.clickedBook = {name: "", author: "", image: "", price: "", rating: "", genre: "", description: ""};
      this.showBookModal = !this.showBookModal;
    },
    closeEditBook: function () {
      this.clickedBook = {name: "", author: "", image: "", price: "", rating: "", genre: "", description: ""};
      this.showBookEditModal = !this.showBookEditModal;
    },
    updateBook: function (updatedBook) {
      this.clickedBook.name = updatedBook.name;
      this.clickedBook.author = updatedBook.author;
      this.clickedBook.image = updatedBook.image;
      this.clickedBook.price = updatedBook.price;
      this.clickedBook.rating = updatedBook.rating;
      this.clickedBook.genre = updatedBook.genre;
      this.clickedBook.description = updatedBook.description;
      this.editBook()
    },
  },
  created: function() {
      this.books = [
          {name: "Rok 1984", author: "George Orwell", image: "https://s.lubimyczytac.pl/upload/books/4978000/4978484/919129-352x500.jpg", price: "10.50", rating: "8.5", genre: "Powieść", description: "Najważniejszą i chyba najsłynniejszą książką Orwella jest 1984. Ta ponura antyutopia rozgrywa się w rządzonym przez totalitarny reżim Wielkiego Brata Londynie, a cały świat podzielony jest na trzy walczące ze sobą bloki państw-supermocarstw. Choć może to wcale nieprawda. Bowiem w świecie 1984 słowa straciły już swoje znaczenie, a dwa plus dwa nie musi już równać się cztery. Wszak główny bohater powieści Winston Smith pracuje w Ministerstwie Prawdy, które zajmuje się manipulacją, propagandą i dezinformacją. A działa jeszcze Ministerstwo Pokoju, które zajmuje się Wojną. Ministerstwo Miłości prześladowaniem obywateli, utrzymywaniem przy pomocy siły porządku, egzekucjami i torturami. A Ministerstwo Obfitości – rozdawaniem głodowych racji dla motłochu."},
          {name: "Pan raczy zartować, panie Feynman!", author: "Richard Feynmann", image: "https://s.lubimyczytac.pl/upload/books/4860000/4860910/686871-352x500.jpg", price: "30.99", rating: "7.9", genre: "Autobiografia", description: "Richard P. Feynman - laureat Nagrody Nobla i jeden z najgenialniejszych fizyków w dziejach w brawurowy sposób opowiada o swoim niezwykłym życiu. Nie tylko przedstawia kulisy prac nad budową bomby atomowej i tajemnicami materii, ale też ujawnia się jako spec od otwierania zamków szyfrowych i mistrz gry na bongosach. Feynman opisuje spotkanie z Einsteinem, walki z rządową biurokracją, codzienne życie fizyków w ośrodku badawczym w Los Alamos i ich flirty z tancerkami w Las Vegas."},
          {name: "Król szczurów", author: "James Clavell", image: "https://s.lubimyczytac.pl/upload/books/178000/178456/143711-352x500.jpg", price: "20.50", rating: "8.1", genre: "Powieść", description: "W 1941 roku, 17-letni James Clavell, został wysłany na wojnę do Malezji. Przez wiele miesięcy ukrywał się w tropikalnej dżungli, żył w malezyjskiej wiosce. Schwytany jednak przez Japończyków trafił do obozu jenieckiego Changi w pobliżu Singapuru. Ze 150 tysięcy więźniów, którzy tu trafili, przeżyło zaledwie 10 tysięcy. Pozostałych wykończyły głód i tropikalne choroby. Clavell przeżył. I właśnie to obozowe doświadczenie stało się kanwą jego debiutanckiej powieści - Króla szczurów. To książka o zachowaniu człowieka w sytuacji ekstremalnej. Ale nawet w piekle można być człowiekiem wolnym i zachować resztki godności - jak dwuznaczny moralnie amerykański kapral, nazywany przez współwięźniów Królem, albo jak kapitan Marlowe, literackie alter ego pisarza, zaznaczmy jednego z najpopularniejszych amerykańskich pisarzy, zwanego MR."}
          ];
      localStorage.setItem('books', JSON.stringify(this.books));
  },
  mounted: function () {
    const existingbooks = localStorage.getItem('books');
    this.books = JSON.parse(existingbooks) || [];
  }
};
</script>

<style>
.books-list{
  position: absolute;
  top: 80px;
  left: 0;
  right: 0;
}
</style>
