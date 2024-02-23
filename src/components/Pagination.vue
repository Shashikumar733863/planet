<template>
  <div class="pagination">
    <button v-if="currentPage > 1" @click="changePage(currentPage - 1)">Prev</button>
    <button v-for="page in pageNumbers" :key="page" :class="{ active: page === currentPage }" @click="changePage(page)">{{ page }}</button>
    <button v-if="currentPage < totalPages" @click="changePage(currentPage + 1)">Next</button>
  </div>
</template>

<script>
export default {
  props: {
    currentPage: {
      type: Number,
      required: true,
    },
    totalPages: {
      type: Number,
      required: true,
    },
  },
  computed: {
    pageNumbers() {
      const visiblePages = 5; // Adjust as needed
      const startPage = Math.max(1, this.currentPage - Math.floor(visiblePages / 2));
      const endPage = Math.min(this.totalPages, startPage + visiblePages - 1);
      return Array.from({ length: endPage - startPage + 1 }, (_, i) => startPage + i);
    },
  },
  methods: {
    changePage(newPage) {
      this.$emit('page-change', newPage);
    },
  },
};
</script>

<style scoped>
.pagination {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 0.5rem;
  font-family: sans-serif; 
}

.pagination button {
  padding: 0.5rem 1rem;
  border: none;
  border-radius: 4px;
  background-color: #f0f0f0;
  cursor: pointer;
  transition: background-color 0.2s ease-in-out;
}

.pagination button.active,
.pagination button:hover {
  background-color: #ddd;
}

.pagination button:focus {
  outline: none;
  box-shadow: 0 0 0 2px #ccc;
}
</style>

  