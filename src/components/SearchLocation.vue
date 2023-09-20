<!-- src/components/SearchLocation.vue -->
<template>
    <!-- Input box and Search button -->
    <div>
      <!-- Data binding with v-model to keep track of search query -->
      <input type="text" v-model="searchQuery" @keyup.enter="searchLocation" />
      <!-- Button to trigger searchLocation method -->
      <a-button @click="searchLocation">Search</a-button>
    </div>
  </template>
  
  <script>
  export default {
    // Declare component's local state
    data() {
      return {
        searchQuery: '' // Holds the search query
      };
    },
    methods: {
      // Method to perform location search
      searchLocation() {
        // Emit the search query to the parent component
        this.$emit('search', this.searchQuery);
      }
    }
  };
  </script>
  