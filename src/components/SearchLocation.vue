<!-- src/components/SearchLocation.vue -->
<template>
    <div>
      <input type="text" v-model="searchQuery" @keyup.enter="searchLocation" />
      <a-button @click="searchLocation">Search</a-button>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        searchQuery: ''
      };
    },
    methods: {
      searchLocation() {
        this.$emit('search', this.searchQuery); // emitting the search query to the parent component
      }
    }
  };
  </script>
  