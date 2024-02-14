<template>
  <div class="books-list">
    <template v-for="(book, index) in booksArray" :key="index">
      <div class="book" v-if="book.work.cover_id">
        <img
          class="book-cover"
          :src="`https://covers.openlibrary.org/b/id/${book.work.cover_id}-M.jpg`"
        />
        <div class="book-content">
          <div>{{ book.work.title }}</div>
          <div>Author: {{ book.work.author_names.join(", ") }}</div>
          <div>Published Year: {{ book.work.first_publish_year }}</div>
        </div>
        <div class="book-button">
          <UButton
            :color="bookStatusColor(index)"
            variant="solid"
            @click="toggleBookStatus(index)"
            >{{ bookStatusText(index) }}</UButton
          >
        </div>
      </div>
    </template>
  </div>
</template>

<script>
export default {
  data() {
    return {
      bookStatuses: Array(this.booksArray.length).fill(false),
    };
  },
  props: {
    booksArray: {
      type: Array,
      required: true,
    },
  },
  methods: {
    toggleBookStatus(index) {
      this.bookStatuses[index] = !this.bookStatuses[index];
    },
    bookStatusColor(index) {
      return this.bookStatuses[index] ? "primary" : "white";
    },
    bookStatusText(index) {
      return this.bookStatuses[index] ? "Read" : "Unread";
    },
  },
};
</script>

<style scoped>
.books-list {
  display: flex;
  flex-wrap: wrap;
  gap: 16px;
  justify-content: center;
}
.book {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 250px;
  gap: 8px;
  border-radius: 4px;
  padding: 8px 0px;
  box-shadow: 0px 1px 4px rgba(255, 255, 255, 0.07);
}
.book-cover {
  max-height: 250px;
}
.book-content {
  padding: 4px 8px;
}
.book-button {
  padding: 8px 0px;
}
</style>
