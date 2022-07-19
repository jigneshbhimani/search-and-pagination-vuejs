<template>
  <div class="overflow-auto">
    <p class="mt-3 title">Current Page: {{ currentPage }}</p>
    <div class="container">
      <input
        type="text"
        placeholder="Search Here..."
        class="mt-3 search"
        v-model="search"
        @keyup="getAllItems"
      />
      <br />
      <br />
      <div class="card">
        <div class="card-body">
          <b-table
            id="my-table"
            :items="items"
            :per-page="perPage"
            :current-page="currentPage"
            large
          ></b-table>
        </div>
      </div>
    </div>
    <br />
    <div class="pagination">
      <b-pagination
        v-model="currentPage"
        :total-rows="rows"
        :per-page="perPage"
        aria-controls="my-table"
      ></b-pagination>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      search: null,
      perPage: 10,
      currentPage: 1,
      items: [],
    };
  },
  computed: {
    rows() {
      return this.items.length;
    },
  },
  methods: {
    getAllItems() {
      axios.get("http://localhost:3000/items").then((response) => {
        if (this.search) {
          this.items = response.data.filter(
            (item) =>
              item.id.toString().includes(this.search.toString()) ||
              item.first_name
                .toLowerCase()
                .includes(this.search.toLowerCase()) ||
              item.last_name
                .toLowerCase()
                .includes(this.search.toLowerCase()) ||
              item.email.toLowerCase().includes(this.search.toLowerCase())
          );
        } else {
          this.items = response.data;
        }
      });
    },
  },
  created() {
    this.getAllItems();
  },
};
</script>

<style>
.pagination {
  margin-left: 23.5rem;
}
.title {
  text-align: center;
}
.search {
  width: 30%;
  border-radius: 10px;
}
</style>
