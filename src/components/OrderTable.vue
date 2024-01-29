<template>
  <div class="mt-2">
    <p>
      Total Amount:<b>{{ totalAmountPage }}</b>
    </p>
  </div>
  <div>
    <table class="table">
      <thead>
        <tr>
          <th>Order name</th>
          <th>Customer Company</th>
          <th>Customer name</th>
          <th>Order date</th>
          <th>Delivered Amount</th>
          <th>Total Amount</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="order in orders" :key="order.id">
          <td>{{ order.order_name }}</td>
          <td>{{ order.customer_company }}</td>
          <td>{{ order.customer_name }}</td>
          <td>{{ formatDate(order.order_date) }}</td>
          <td>
            <b>{{ formatCurrency(order.delivered_amount) }}</b>
          </td>
          <td>
            <b>{{ formatCurrency(order.total_amount) }}</b>
          </td>
        </tr>
      </tbody>
    </table>
    <nav aria-label="Page navigation example">
      <ul class="pagination justify-content-center">
        <li class="page-item">
          <span class="page-link">Total {{ totalPages }}</span>
        </li>
        <li class="page-item" :class="{ disabled: currentPage <= 1 }">
          <a
            class="page-link"
            href="#"
            aria-label="Previous"
            @click="changePage(currentPage - 1)"
          >
            <span aria-hidden="true">&laquo;</span>
          </a>
        </li>
        <li class="page-item" :class="{ active: true }">
          <span class="page-link">{{ currentPage }}</span>
        </li>
        <li class="page-item" :class="{ disabled: currentPage >= totalPages }">
          <a
            class="page-link"
            href="#"
            aria-label="Next"
            @click="changePage(currentPage + 1)"
          >
            <span aria-hidden="true">&raquo;</span>
          </a>
        </li>
        <li class="page-item">
          <input
            type="number"
            class="page-link"
            min="1"
            :max="totalPages"
            v-model.number="jumpPage"
            @keyup.enter="changePage(jumpPage)"
            placeholder="Go to"
          />
        </li>
      </ul>
    </nav>
  </div>
</template>

<script>
export default {
  props: {
    orders: Array,
    currentPage: Number,
    totalPages: Number,
  },
  data() {
    return {
      jumpPage: "", // Awalnya kosong agar placeholder terlihat
    };
  },
  methods: {
    changePage(page) {
      const targetPage = Number(page);
      if (targetPage > 0 && targetPage <= this.totalPages) {
        this.$emit("page-changed", targetPage);
      }
    },
    formatDate(dateString) {
      const date = new Date(dateString);
      const options = {
        year: "numeric",
        month: "short",
        day: "numeric",
        hour: "2-digit",
        minute: "2-digit",
        hour12: true,
      };
      return date.toLocaleString("en-US", options).replace(",", " ");
    },
    formatCurrency(amount) {
      if (amount === 0) {
        return "-";
      }
      return new Intl.NumberFormat("en-US", {
        style: "currency",
        currency: "USD",
      }).format(amount);
    },
  },
  computed: {
    totalAmountPage() {
      return this.formatCurrency(
        this.orders.reduce(
          (total, order) => total + parseFloat(order.total_amount),
          0
        )
      );
    },
  },
};
</script>

<style scoped>
.pagination {
  margin-top: 1rem;
}

.page-link {
  color: #495057;
}

.page-item.active .page-link {
  background-color: #f0f1f5;
  border-color: #dee2e6;
}

.page-item .page-link {
  color: #6c757d;
}

.table {
  margin-top: 1rem;
  background-color: white;
}

.table th {
  background-color: #f8f9fa;
  border-top: none;
}

.table td {
  border-top: 1px solid #dee2e6;
}
</style>
