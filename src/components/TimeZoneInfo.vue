<!-- src/components/TimeZoneInfo.vue -->
<template>
    <div class="time-zone-container">
      <p class="time-zone-label"><strong>Time Zone:</strong> {{ timeZone || 'Not available' }}</p>
      <p class="local-time-label"><strong>Local Time:</strong> {{ localTime || 'Not available' }}</p>
    </div>
  </template>
  
  <script>
  export default {
    props: ['timeZone', 'localTime']
  };
  </script>
  <style scoped>
  .time-zone-container {
    background-color: #f7f7f7;
    border-radius: 8px;
    padding: 16px;
    box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.1);
    margin-top: 16px;
  }
  
  .time-zone-label,
  .local-time-label {
    color: black;
    margin: 8px 0;
  }
  </style>