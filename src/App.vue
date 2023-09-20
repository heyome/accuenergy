<!-- src/App.vue -->
<template>
  <div id="app" class="container">
    <div class="row">
      
      <SearchLocation class="search-box" @search="searchForLocation" />
    </div>

    <div class="row">
      <MyMap :locations="locations" :lastLocation="lastLocation" />
    </div>

    <div class="row">
      <GetPositionButton class="btn"
        :lastLocation="lastLocation"
        @update:lastLocation="lastLocation = $event"
      />
      <TimeZoneInfo :timeZone="timeZone" :localTime="localTime" />
    </div>

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
      locations: [],
      lastLocation: {},
      timeZone: '',
      localTime: ''
    };
  },
  methods: {
    async searchForLocation(query) {
      const apiKey = 'AIzaSyCeLrlgN0gjHy77Cpu-Il-BHiyFGSQkbR8';
      try {
        const response = await axios.get(`https://maps.googleapis.com/maps/api/geocode/json?address=${query}&key=${apiKey}`);
        const currentTimestamp = Math.floor(Date.now() / 1000);
        if (response.data.results[0]) {
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
      if (this.lastLocation) {
      try {
        const currentTimestamp = Math.floor(Date.now() / 1000);
        const response = await axios.get(`https://maps.googleapis.com/maps/api/timezone/json?location=${this.lastLocation.lat},${this.lastLocation.lng}&timestamp=${currentTimestamp}&key=${apiKey}`);
        this.timeZone = response.data.timeZoneId;
        this.localTime = new Intl.DateTimeFormat('en-US', { timeZone:this.timeZone, timeStyle: 'medium', dateStyle: 'medium' }).format(new Date());

      } catch (error) {
        console.error('Error getting time zone:', error);
      }
    }
    },
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


