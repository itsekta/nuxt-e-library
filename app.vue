<template>
  <UHorizontalNavigation
    :links="links"
    class="border-b border-gray-200 dark:border-gray-800"
  />

  <div class="main-app">
    <div class="main-content">
      <SearchBar @search="handleSearch" />
      <div class="main-heading">Available Books</div>
      <BooksMiniCard v-if="booksArray" :booksArray="booksArray" />
      <UProgress v-else animation="carousel" style="width: 400px" />
    </div>
  </div>
</template>

<script>
import SearchBar from "./components/SearchBar.vue";
import BooksMiniCard from "./components/BooksMiniCard.vue";
export default {
  data() {
    return {
      links: [
        {
          label: "E-Library",
          icon: "basil:book-open-solid",
          to: "/",
        },
      ],
      booksArray: null,
    };
  },
  components: {
    SearchBar,
    BooksMiniCard,
  },
  mounted() {
    this.fetchBooks();
  },
  methods: {
    async fetchBooks() {
      const booksReq = await $fetch(
        "https://openlibrary.org/people/mekBot/books/already-read.json"
      );
      console.log(booksReq);
      this.booksArray = booksReq.reading_log_entries;
    },
    async handleSearch(payload) {
      try {
        const searchReq = await $fetch(
          `https://openlibrary.org/search.json?q=${payload}`,
          {
            method: "GET",
          }
        );
        console.log(payload);
        console.log(searchReq);
      } catch (error) {
        console.log("Search Api Failed", error);
      }
    },
  },
};
</script>

<style scoped>
.main-app {
  padding: 16px;
  color: #f4f4f4;
}
.main-content {
  display: flex;
  flex-direction: column;
  gap: 16px;
  align-items: center;
}
.main-heading {
  font-size: 20px;
}
</style>
