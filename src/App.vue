<template>
  <div id="app" class="container">
    <!-- Search Location Box -->
    <div class="row">
      <SearchLocation class="search-box" @search="searchForLocation" />
    </div>

    <!-- Map Display -->
    <div class="row">
      <MyMap :locations="locations" :lastLocation="lastLocation" />
    </div>

    <!-- Position and Time Zone Information -->
    <div class="row">
      <GetPositionButton class="btn"
        :lastLocation="lastLocation"
        @update:lastLocation="lastLocation = $event"
      />
      <TimeZoneInfo :timeZone="timeZone" :localTime="localTime" />
    </div>

    <!-- Table of Searched Locations -->
    <div class="row">
      <SearchedLocationsTable :locations="locations" @deleteSelected="deleteSelectedRows" />
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import GetPositionButton from './components/GetPositionButton.vue';
import SearchLocation from './components/SearchLocation.vue';
import MyMap from './components/MyMap.vue';
import SearchedLocationsTable from './components/SearchedLocationsTable.vue';
import TimeZoneInfo from './components/TimeZoneInfo.vue';

export default {
  components: {
    GetPositionButton,
    SearchLocation,
    MyMap,
    TimeZoneInfo,
    SearchedLocationsTable,
  },
  data() {
    return {
      locations: [], // Holds the locations that have been searched
      lastLocation: {}, // Holds the last searched or current location
      timeZone: '', // Holds the time zone information
      localTime: '' // Holds the local time
    };
  },
  methods: {
    // Search for a location based on the input query
    async searchForLocation(query) {
      const apiKey = 'AIzaSyCeLrlgN0gjHy77Cpu-Il-BHiyFGSQkbR8';
      try {
        // API call to get location details
        const response = await axios.get(`https://maps.googleapis.com/maps/api/geocode/json?address=${query}&key=${apiKey}`);
        const currentTimestamp = Math.floor(Date.now() / 1000);
        
        if (response.data.results[0]) {
          // Save the location details
          const location = {
            name: query,
            lat: response.data.results[0].geometry.location.lat,
            lng: response.data.results[0].geometry.location.lng,
            key: currentTimestamp
          };

          this.locations.push(location);
          this.lastLocation = location;
        } else {
          console.error('No location found for the given query.');
        }
      } catch (error) {
        console.error('An error occurred while searching for location:', error);
      }

      // Get time zone and local time for the last location
      if (this.lastLocation) {
        // API call to get time zone information
        try {
          const currentTimestamp = Math.floor(Date.now() / 1000);
          const response = await axios.get(`https://maps.googleapis.com/maps/api/timezone/json?location=${this.lastLocation.lat},${this.lastLocation.lng}&timestamp=${currentTimestamp}&key=${apiKey}`);
          this.timeZone = response.data.timeZoneId;
          this.localTime = new Intl.DateTimeFormat('en-US', { timeZone: this.timeZone, timeStyle: 'medium', dateStyle: 'medium' }).format(new Date());
        } catch (error) {
          console.error('Error getting time zone:', error);
        }
      }
    },

    // Delete selected rows from the table
    deleteSelectedRows(selectedLocations) {
      this.locations = this.locations.filter(loc => !selectedLocations.includes(loc));
    },
  }
};
</script>

<style>
.container {
  max-width: 1200px;
  margin: auto;
  padding: 20px;
}

.row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}

.btn {
  background-color: #007bff;
  color: white;
  padding: 10px 20px;
  border: none;
  cursor: pointer;
}

.search-box {
  flex-grow: 1;
  margin-left: 20px;
}

</style>
