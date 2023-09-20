<!-- Component Template -->
<template>
    <!-- Main Container -->
    <div class="container">
      <!-- Table to Display Location Data -->
      <table class="styled-table">
        <thead>
          <tr>
            <!-- Table Headers -->
            <th>Select</th>
            <th>Name</th>
            <th>Latitude</th>
            <th>Longitude</th>
          </tr>
        </thead>
        <tbody>
          <!-- Loop through paginated locations and render them -->
          <tr v-for="loc in paginatedLocations" :key="loc.name">
            <td>
              <!-- Checkbox to Select Row -->
              <input type="checkbox" v-model="loc.selected" />
            </td>
            <td>{{ loc.name }}</td>
            <td>{{ loc.lat }}</td>
            <td>{{ loc.lng }}</td>
          </tr>
        </tbody>
      </table>
      <!-- Pagination Buttons -->
      <button class="btn-secondary" @click="previousPage">Previous</button>
      <button class="btn-secondary" @click="nextPage">Next</button>
      <!-- Button to Trigger Deletion -->
      <button class="btn-primary" @click="deleteSelectedRows">Delete Selected</button>
    </div>
  </template>
  
  <script>
  // Component Script
  export default {
    // Accepting 'locations' as a prop from parent component
    props: ['locations'],
    // Local Component State
    data() {
      return {
        currentPage: 0,
        perPage: 10,
      };
    },
    // Computed Property for Pagination
    computed: {
      paginatedLocations() {
        const start = this.currentPage * this.perPage;
        const end = start + this.perPage;
        // Slice the array to only include locations for the current page
        return this.locations.slice(start, end);
      },
    },
    // Component Methods
    methods: {
      // Delete selected rows
      deleteSelectedRows() {
        // Emitting custom event to parent component
        this.$emit("deleteSelected", this.locations.filter(loc => loc.selected));
      },
      // Navigate to the next page
      nextPage() {
        // Check to avoid going past the last page
        if ((this.currentPage + 1) * this.perPage < this.locations.length) {
          this.currentPage++;
        }
      },
      // Navigate to the previous page
      previousPage() {
        // Check to avoid going below the first page
        if (this.currentPage > 0) {
          this.currentPage--;
        }
      },
    },
  };
  </script>
  
  <!-- Component Styles -->
  <style>
    /* Container Style */
    .container {
      margin: 20px;
      text-align: center;
    }
    
    /* Primary Button Style */
    .btn-primary {
      background-color: #007bff;
      color: white;
      padding: 10px 20px;
      margin: 10px;
      border: none;
      cursor: pointer;
    }
    
    /* Secondary Button Style */
    .btn-secondary {
      background-color: #ccc;
      color: black;
      padding: 10px 20px;
      margin: 10px;
      border: none;
      cursor: pointer;
    }
    
    /* Table Styles */
    .styled-table {
      width: 100%;
      margin: 25px 0;
      font-size: 0.9em;
      text-align: left;
      border-collapse: collapse;
    }
    
    /* Table Cell and Header Styles */
    .styled-table th, 
    .styled-table td {
      padding: 12px 15px;
    }
    
    /* Table Header Styles */
    .styled-table thead {
      background-color: #009879;
      color: white;
      border-bottom: solid 3px #006464;
    }
    
    /* Table Row Styles */
    .styled-table tbody tr {
      border-bottom: thin solid #dddddd;
    }
    
    /* Alternate Row Color */
    .styled-table tbody tr:nth-of-type(even) {
      background-color: #f3f3f3;
    }
  
    /* Remove Border from Last Row */
    .styled-table tbody tr:last-of-type {
      border-bottom: none;
    }
  </style>
  