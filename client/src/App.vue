<template>
<main>
  <h1>Hotel Code</h1>
  <booking-form></booking-form>
  <bookings-grid v-if="bookings" :bookings="bookings"></bookings-grid>
</main>


</template>

<script>
import { eventBus } from './main.js';
import BookingService from './services/BookingService.js';
import BookingsGrid from './components/BookingsGrid.vue';
import BookingForm from './components/BookingsForm.vue';

export default {
  name: 'App',
  components: {
    "bookings-grid": BookingsGrid,
    "booking-form": BookingForm
  },
  data () {
    return {
      bookings: []
    }
  },
  mounted() {
    this.showBookings()

    eventBus.$on('submit-booking', payload => {
      BookingService.postBooking(payload)
      .then(booking => this.bookings.push(booking));
    });

    eventBus.$on('delete-booking', id => {
      BookingService.deleteBooking(id)
      .then(() => {
        const index = this.bookings.findIndex(booking =>
        booking._id === id);
        this.bookings.splice(index, 1);
      });
    });

  },
  methods: {
    showBookings() {
      BookingService.getBookings()
      .then(bookings => this.bookings = bookings)
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
