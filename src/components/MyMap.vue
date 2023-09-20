<template>
    <div id="map" style="width: 100%; height: 400px;"></div>
  </template>
  
  <script>
  /* global google */
  import { shallowRef, ref, watch, onMounted } from 'vue';
  
  
  export default {
    props: ['lastLocation','locations'],
    setup(props) {
      const map = ref(null);
  
      onMounted(() => {
        const mapOptions = {
          center: { lat: 43.653226, lng: -79.3831843 },
          zoom: 8
        };
    
        map.value = new google.maps.Map(document.getElementById('map'), mapOptions);
      });

      // Declare a reactive ref to hold markers
      const markers = shallowRef([]);
  
      watch(
        () => props.lastLocation,
        (newLastLocation) => {
            console.log(props.lastLocation)

          // Clear existing markers and add new markers.
          props.locations.forEach((location) => {
            const marker = new google.maps.Marker({
                position: location,
                map: map.value,
            });
            markers.value.push(marker);
          });
          // Re-center the map to the last location.
          if (props.locations.length > 0) {
            console.log("Re-center the map to the last location.")
            map.value.setCenter(newLastLocation);
          }
        },
        { immediate: true }  // This makes sure the handler is called immediately when the watcher is established.
      );

      

watch(
  () => props.locations,
  (newLocations) => {

    // Clear existing markers
    markers.value.forEach(marker => marker.setMap(null));
    markers.value = [];

    // Add new markers and store them in markers ref
    newLocations.forEach((location) => {
      const marker = new google.maps.Marker({
        position: location,
        map: map.value,
      });
      markers.value.push(marker);
    });

    // Re-center the map to the last location.
    if (newLocations.length > 0) {
        console.log(newLocations)
      map.value.setCenter(newLocations[0]);
    }
  },
  { immediate: true }
)


    }
  }
  </script>