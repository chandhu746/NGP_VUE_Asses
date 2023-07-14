<template>
    <div>
      <button @click="openForm" class="add-event-btn">Add Event</button>
      <div v-if="isFormOpen" class="event-form">
        <h2>Add Event</h2>
        <form @submit.prevent="createEvent">
          <div class="form-group">
            <label for="title">Event Title:</label>
            <input type="text" id="title" v-model="eventData.title" required>
          </div>
          <div class="form-group">
            <label for="date">Event Date:</label>
            <input type="date" id="date" v-model="eventData.date" required>
          </div>
          <div class="form-group">
            <label for="time">Event Time:</label>
            <input type="time" id="time" v-model="eventData.time" required>
          </div>
          <div class="form-group">
            <label for="place">Event Place:</label>
            <input type="text" id="place" v-model="eventData.place" required>
          </div>
          <button type="submit" class="create-event-btn">Create Event</button>
        </form>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        isFormOpen: false,
        eventData: {
          title: '',
          date: '',
          time: '',
          place: ''
        }
      };
    },
    methods: {
      openForm() {
        this.isFormOpen = true;
      },
      createEvent() {
        // Emit the event with the eventData
        this.$emit('event-created', this.eventData);
        // Reset the form fields
        this.eventData = {
          title: '',
          date: '',
          time: '',
          place: ''
        };
        // Close the form
        this.isFormOpen = false;
      }
    }
  };
  </script>
  
  <style>
  .add-event-btn {
    background-color: #4caf50;
    color: white;
    padding: 10px 20px;
    border: none;
    cursor: pointer;
    font-size: 16px;
  }
  
  .event-form {
    margin-top: 20px;
  }
  
  .form-group {
    margin-bottom: 10px;
  }
  
  label {
    display: block;
    font-weight: bold;
  }
  
  input[type="text"],
  input[type="date"],
  input[type="time"] {
    width: 100%;
    padding: 5px;
    font-size: 14px;
    border: 1px solid #ccc;
    border-radius: 4px;
  }
  
  .create-event-btn {
    background-color: #4caf50;
    color: white;
    padding: 10px 20px;
    border: none;
    cursor: pointer;
    font-size: 16px;
  }
  </style>
  