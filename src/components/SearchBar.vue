<template>
  <div class="search-bar-container">
    <div class="search-bar">
      <div class="search-filter">
        <button @click="search" class="btn btn-primary">
          <i class="fa-solid fa-magnifying-glass"></i> Search
        </button>
        <input
          type="text"
          v-model="searchQuery"
          placeholder="Search"
          class="form-control search-input"
          @keyup.enter="search"
        />
      </div>
      <h5>Created date</h5>
      <div class="date-filter">
        <label for="dateRange" class="form-control date-label">
          <i class="date-icon">📅</i>
          <span class="date-range-text">Start date - End date </span>
          <input
            type="date"
            v-model="startDate"
            class="date-range-input"
            placeholder="Start date"
          />
          <span class="date-range-separator"> - </span>
          <input
            type="date"
            v-model="endDate"
            class="date-range-input"
            placeholder="End date"
          />
        </label>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      searchQuery: "",
      startDate: "",
      endDate: "",
    };
  },
  methods: {
    search() {
      this.$emit("search", {
        query: this.searchQuery,
        startDate: this.startDate,
        endDate: this.endDate,
      });
    },
  },
};
</script>

<style scoped>
.form-control.search-input {
  /* Style untuk input pencarian */
  width: auto;
}

.date-filter {
  position: relative;
  /* Sesuaikan style lain jika diperlukan */
}

.date-label {
  display: table-cell;
  align-items: center;
  gap: 10px; /* Atur jarak antar elemen */
  padding: 5px;
  border: 1px solid #ced4da; /* Warna border input */
  border-radius: 0.25rem; /* Bulatkan sudut */
}

.date-range-text {
  font-size: 0.875rem; /* Sesuaikan ukuran font */
  color: #6c757d; /* Warna teks placeholder */
}

.date-range-input {
  border: none;
  outline: none;
  width: auto; /* Atur lebar agar cukup untuk tanggal */
  padding: 0; /* Hilangkan padding */
  margin: 0; /* Hilangkan margin */
  background-color: transparent; /* Buat latar belakang transparan */
}

.date-range-separator {
  color: #6c757d; /* Warna teks separator */
}

.btn-primary {
  /* Style untuk tombol pencarian */
  /* Style untuk tombol Search */
  border: none;
  background-color: #fff;
  color: #495057;
  padding: 0.375rem 0.75rem;
  margin-right: -1px; /* Hilangkan gap antara tombol dan input */
  border-right: 1px solid #ced4da; /* Tambahkan pemisah */
  cursor: pointer;
}

.search-input-group {
  display: flex;
  flex-grow: 1; /* Input group akan memenuhi ruang yang tersedia */
}

.search-input {
  /* Style untuk input pencarian */
  flex-grow: 1; /* Input akan memenuhi ruang yang tersedia */
  padding: 0.375rem 0.75rem;
}

.search-input:focus {
  /* Style saat input difokuskan */
  outline: none;
}

.search-filter {
  display: flex;
}
</style>
