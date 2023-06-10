<template>
  <div>
    <input type="text" v-model="searchQuery" placeholder="Search products..." />
    <table class="table table-bordered table-striped w-100">
      <thead>
        <tr>
          <th>Thumbnail</th>
          <th>Name</th>
          <th>Description</th>
          <th>Price</th>
          <th>Discount</th>
        </tr>
      </thead>
      <tbody>
        <ProductRow
          v-for="product in products"
          :key="product.id"
          :product="product"
        />
      </tbody>
    </table>

    <nav>
      <ul class="pagination">
        <li class="page-item" :class="{ disabled: currentPage === 1 }">
          <a class="page-link" href="#" @click="prevPage">Previous</a>
        </li>
        <li
          class="page-item"
          v-for="page in totalPages"
          :key="page"
          :class="{ active: page === currentPage }"
        >
          <a class="page-link" href="#" @click="gotoPage(page)">{{ page }}</a>
        </li>
        <li class="page-item" :class="{ disabled: currentPage === totalPages }">
          <a class="page-link" href="#" @click="nextPage">Next</a>
        </li>
      </ul>
    </nav>
  </div>
</template>

<script>
import axios from "axios";
import ProductRow from "./components/ProductRow.vue";

export default {
  components: {
    ProductRow,
  },
  data() {
    return {
      products: [],
      searchQuery: "",
      searchTimeout: null,
      currentPage: 1,
      pageSize: 10,
    };
  },
  computed: {
    totalPages() {
      return Math.ceil(this.products.length / this.pageSize);
    },
    paginatedProducts() {
      const startIndex = (this.currentPage - 1) * this.pageSize;
      const endIndex = startIndex + this.pageSize;
      return this.products.slice(startIndex, endIndex);
    },
  },
  mounted() {
    this.fetchProducts();
  },
  watch: {
    searchQuery(newValue) {
      clearTimeout(this.searchTimeout);
      this.searchTimeout = setTimeout(() => {
        this.currentPage = 1; // Reset the current page when the search query changes
        this.fetchProducts(newValue);
      }, 500);
    },
  },
  methods: {
    fetchProducts(searchQuery = "") {
      const params = searchQuery ? { q: searchQuery } : {};
      let url = "https://dummyjson.com/products";
      if (searchQuery) {
        url = url.concat("/search");
      }
      axios
        .get(url, { params })
        .then((response) => {
          this.products = response.data.products;
        })
        .catch((error) => {
          console.error(error);
        });
    },
    prevPage() {
      if (this.currentPage > 1) {
        this.currentPage--;
      }
    },
    nextPage() {
      if (this.currentPage < this.totalPages) {
        this.currentPage++;
      }
    },
    gotoPage(page) {
      this.currentPage = page;
    },
  },
};
</script>
