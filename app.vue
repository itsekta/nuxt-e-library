<template>
  <Head>
    <Title>E-Library</Title>
  </Head>
  <UHorizontalNavigation
    :links="links"
    class="border-b border-gray-200 dark:border-gray-800"
  />

  <div class="main-app">
    <div class="main-content">
      <SearchBar @search="handleSearch" />
      <div class="main-heading">List of Available Books</div>
      <UProgress v-if="isLoading" animation="carousel" style="width: 400px" />
      <div v-else>
        <BooksMiniCard v-if="booksArray" :booksArray="booksArray" />
      </div>
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
      isLoading: false,
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
      this.isLoading = true;
      try {
        const booksReq = await $fetch(
          "https://openlibrary.org/people/mekBot/books/already-read.json"
        );
        console.log(booksReq);
        this.booksArray = booksReq.reading_log_entries;
        this.isLoading = false;
      } catch (error) {
        console.log("Fetching API failed");
      }
    },
    async handleSearch(payload) {
      this.isLoading = true;
      try {
        const searchReq = await $fetch(
          `https://openlibrary.org/search.json?q=${payload}`
        );
        console.log(payload);
        console.log(searchReq);
        const formattedBookData = searchReq.docs.map((book) => ({
          work: {
            title: book.title,
            author_names: book.author_name,
            cover_id: book.cover_i,
            first_publish_year: book.first_publish_year,
          },
        }));

        console.log(formattedBookData);
        this.booksArray = formattedBookData;
        this.isLoading = false;
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
