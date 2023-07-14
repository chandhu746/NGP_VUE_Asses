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
            <input type="datetime-local" id="date" v-model="eventData.eventDate" required>
          </div>
          <div class="form-group">
            <label for="time">Event Time:</label>
            <input type="text" id="time" v-model="eventData.time" required>
          </div>
          <div class="form-group">
            <label for="place">Event Place:</label>
            <input type="text" id="place" v-model="eventData.place" required>
          </div>
          <button type="submit" class="create-event-btn">Create Event</button>
        </form>
      </div>
      <table>
        <thead>
          <tr>
            <th>Title</th>
            <th>Date</th>
            <th>Time</th>
            <th>Place</th>
            <th>Actions</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="event in events" :key="event.id">
            <td>{{ event.title }}</td>
            <td>{{ event.eventDate }}</td>
            <td>{{ event.time }}</td>
            <td>{{ event.place }}</td>
            <td>
              <button @click="deleteEvent(event.id)">Delete</button>
              <button @click="openEditForm(event)">Edit</button>
            </td>
          </tr>
        </tbody>
      </table>
      <div v-if="isEditFormOpen" class="event-form">
        <h2>Edit Event</h2>
        <form @submit.prevent="updateEvent">
          <div class="form-group">
            <label for="edit-title">Event Title:</label>
            <input type="text" id="edit-title" v-model="selectedEvent.title" required>
          </div>
          <div class="form-group">
            <label for="edit-date">Event Date:</label>
            <input type="datetime-local" id="edit-date" v-model="selectedEvent.eventDate" required>
          </div>
          <div class="form-group">
            <label for="edit-time">Event Time:</label>
            <input type="text" id="edit-time" v-model="selectedEvent.time" required>
          </div>
          <div class="form-group">
            <label for="edit-place">Event Place:</label>
            <input type="text" id="edit-place" v-model="selectedEvent.place" required>
          </div>
          <button type="submit" class="update-event-btn">Update Event</button>
        </form>
      </div>
    </div>
  </template>
  
  <script>
  import { defineComponent } from 'vue';
  import axios from 'axios';
  
  export default defineComponent({
    data() {
      return {
        events: [],
        isEditFormOpen: false,
        selectedEvent: {
          id: '',
          title: '',
          eventDate: '',
          time: '',
          place: '',
        },
        isFormOpen: false,
        eventData: {
          title: '',
          evntDate: '',
          time: '',
          place: '',
        },
      };
    },
    mounted() {
      this.getEvents();
    },
    methods: {
      getEvents() {
        axios
          .get('http://127.0.0.1:3333/infoEvent')
          .then((response) => {
            this.events = response.data;
            console.log(this.events);
          })
          .catch((error) => {
            console.error('Error while getting data', error);
          });
      },
      addEvent(eventData) {
        axios
          .post('http://127.0.0.1:3333/insert', eventData)
          .then((response) => {
            this.events.push(response.data);
          })
          .catch((error) => {
            console.error('Error while adding event', error);
          });
      },
      openForm() {
        this.isFormOpen = true;
      },
      createEvent() {
        this.addEvent(this.eventData);
        this.resetForm();
        this.isFormOpen = false;
      },
      openEditForm(event) {
        this.selectedEvent.id = event.id;
        this.selectedEvent.title = event.title;
        this.selectedEvent.date = event.date;
        this.selectedEvent.time = event.time;
        this.selectedEvent.place = event.place;
        this.isEditFormOpen = true;
      },
      updateEvent() {
        axios
          .put(`http://127.0.0.1:3333/up/${this.selectedEvent.id}`, this.selectedEvent)
          .then((response) => {
            const updatedEventIndex = this.events.findIndex((event) => event.id === this.selectedEvent.id);
            if (updatedEventIndex !== -1) {
              this.events.splice(updatedEventIndex, 1, response.data);
            }
            this.isEditFormOpen = false;
          })
          .catch((error) => {
            console.error('Error while updating event', error);
          });
      },
      deleteEvent(id) {
        axios
          .delete(`http://127.0.0.1:3333/delete/${id}`)
          .then((response) => {
            console.log('Data deleted successfully', response.data);
            window.alert('Data is going to be deleted');
            this.getEvents();
          })
          .catch((error) => {
            console.error('Error while deleting event', error);
          });
      },
      resetForm() {
        this.eventData = {
          title: '',
          date: '',
          time: '',
          place: '',
        };
      },
    },
  });
  </script>
  
  <style scoped>
  .event-form {
    margin-top: 20px;
  }

  .add-event-btn {
    background-color: #4caf50;
    color: white;
    padding: 10px 20px;
    border: none;
    cursor: pointer;
    font-size: 16px;
    margin-bottom: 20px;
  }

  .create-event-btn,
  .update-event-btn {
    background-color: #4caf50;
    color: rgb(27, 18, 18);
    padding: 10px 20px;
    border: none;
    cursor: pointer;
    font-size: 16px;
  }

  table {
    width: 100%;
    border-collapse: collapse;
    margin-bottom: 20px;
  }

  th,
  td {
    padding: 8px;
    text-align: left;
    border-bottom: 1px solid #897979;
  }

  th {
    background-color: #050404;
  }
</style>
