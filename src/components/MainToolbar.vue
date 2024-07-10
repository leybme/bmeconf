<template>
    <div class="toolbar">
      <button @click="refreshData">Refresh</button>
      <!-- Add more toolbar buttons or functionalities as needed -->
    </div>
  </template>
  
  <script>
  export default {
    name: 'MainToolbar',
    methods: {
      refreshData() {
        this.$emit('refresh');
      }
    }
  };
  </script>
  
  <style scoped>
  .toolbar {
    background-color: #f5f5f5;
    padding: 10px;
    border-bottom: 1px solid #ddd;
    display: flex;
    justify-content: flex-start;
  }
  
  .toolbar button {
    margin-right: 10px;
  }
  </style>
  