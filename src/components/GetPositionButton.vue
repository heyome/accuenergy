<!-- src/components/GetPositionButton.vue -->
<template>
  <div class="position-container">
    <a-button class="get-button" type="primary" @click="getLocation">
      Get Current Location
    </a-button>
    <div class="info">
      <p class="coordinate-label">
        <strong>Latitude:</strong> {{ location ? location.lat : 'Not available' }}
      </p>
      <p class="coordinate-label">
        <strong>Longitude:</strong> {{ location ? location.lng : 'Not available' }}
      </p>
    </div>
  </div>
</template>


<script>
export default {
  name: 'GetPositionButton',
  props: ['lastLocation'],
  data() {
    return {
      location: null,
    };
  },
  methods: {
    getLocation() {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(
          (position) => {
            this.location = {
              lat: position.coords.latitude,
              lng: position.coords.longitude,
            };
            // Emit an event to update lastLocation in the parent component
            this.$emit('update:lastLocation', this.location);
          },
          (error) => {
            console.error("An error occurred: ", error);
          }
        );
      } else {
        console.error("Geolocation is not supported by this browser.");
      }
    },
  },
};
</script>

<style scoped>
.position-container {
  background-color: #f7f7f7;
  border-radius: 8px;
  padding: 16px;
  box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.1);
}

.get-button {
  margin-bottom: 16px;
}

.info {
  background-color: #eaeaea;
  padding: 12px;
  border-radius: 4px;
}

.coordinate-label {
  color: black;
}
</style>





