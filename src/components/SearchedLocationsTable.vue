<template>
    <div class="container">
      
      <table class="styled-table">
        <thead>
          <tr>
            <th>Select</th>
            <th>Name</th>
            <th>Latitude</th>
            <th>Longitude</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="loc in paginatedLocations" :key="loc.name">
            <td><input type="checkbox" v-model="loc.selected" /></td>
            <td>{{ loc.name }}</td>
            <td>{{ loc.lat }}</td>
            <td>{{ loc.lng }}</td>
          </tr>
        </tbody>
      </table>
      <button class="btn-secondary" @click="previousPage">Previous</button>
      <button class="btn-secondary" @click="nextPage">Next</button>
      <button class="btn-primary" @click="deleteSelectedRows">Delete Selected</button>
    </div>
  </template>

<script>
export default {
  props: ['locations'],
  data() {
    return {
      currentPage: 0,
      perPage: 10,
    };
  },
  computed: {
    paginatedLocations() {
      const start = this.currentPage * this.perPage;
      const end = start + this.perPage;
      return this.locations.slice(start, end);
    },
  },
  methods: {
    deleteSelectedRows() {
      this.$emit("deleteSelected", this.locations.filter(loc => loc.selected));
    },
    nextPage() {
      if ((this.currentPage + 1) * this.perPage < this.locations.length) {
        this.currentPage++;
      }
    },
    previousPage() {
      if (this.currentPage > 0) {
        this.currentPage--;
      }
    },
  },
};
</script>
<style>
  .container {
    margin: 20px;
    text-align: center;
  }

  .btn-primary {
    background-color: #007bff;
    color: white;
    padding: 10px 20px;
    margin: 10px;
    border: none;
    cursor: pointer;
  }

  .btn-secondary {
    background-color: #ccc;
    color: black;
    padding: 10px 20px;
    margin: 10px;
    border: none;
    cursor: pointer;
  }

  .styled-table {
    width: 100%;
    margin: 25px 0;
    font-size: 0.9em;
    text-align: left;
    border-collapse: collapse;
  }

  .styled-table th, 
  .styled-table td {
    padding: 12px 15px;
  }

  .styled-table thead {
    background-color: #009879;
    color: white;
    border-bottom: solid 3px #006464;
  }

  .styled-table tbody tr {
    border-bottom: thin solid #dddddd;
  }

  .styled-table tbody tr:nth-of-type(even) {
    background-color: #f3f3f3;
  }
  
  .styled-table tbody tr:last-of-type {
    border-bottom: none;
  }
</style>