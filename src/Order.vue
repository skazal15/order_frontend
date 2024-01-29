<template>
  <div class="container mt-5">
    <SearchBar @search="handleSearch" />
    <OrdersTable
      :orders="orders"
      :currentPage="currentPage"
      :totalPages="totalPages"
      @page-changed="fetchOrders"
    />
  </div>
</template>

<script>
import axios from "axios";
import SearchBar from "./components/SearchBar.vue";
import OrdersTable from "./components/OrderTable.vue";

export default {
  components: {
    SearchBar,
    OrdersTable,
  },
  data() {
    return {
      orders: [],
      currentPage: 1,
      totalPages: 0, // Total pages will be set based on API response
      searchQuery: "",
      startDate: "",
      endDate: "",
    };
  },
  methods: {
    fetchOrders(pageNumber = this.currentPage) {
      let url = "http://localhost:5400/order/";
      let params = {};

      if (this.searchQuery.startsWith("PO #")) {
        url += "search";
        params = { pageNumber: pageNumber, order: this.searchQuery };
      } else if (this.searchQuery) {
        url += "search";
        params = { pageNumber: pageNumber, product: this.searchQuery };
      } else if (this.startDate && this.endDate) {
        url += "date";
        params = {
          pageNumber: pageNumber,
          start: this.startDate,
          end: this.endDate,
        };
      } else {
        params = { pageNumber };
      }
      axios
        .get(url, { params })
        .then((response) => {
          this.orders = response.data.records;
          this.totalPages = response.data.total_page;
          this.currentPage = pageNumber;
        })
        .catch((error) => {
          console.error("There was an error fetching the orders:", error);
        });
    },
    handleSearch({ query, startDate, endDate }) {
      this.searchQuery = query;
      this.startDate = startDate;
      this.endDate = endDate;
      this.fetchOrders(1); // Fetch with updated search criteria
    },
  },
  created() {
    this.fetchOrders();
  },
};
</script>
