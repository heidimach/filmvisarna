<template>
  <div class="my-bookings white">
    <div class="outer-container white black-text" @click="toggleInfo" v-if="!isHidden">
      <div class="col s3 image-container">
        <img :src="movie.image" alt class="responsive-img image" />
      </div>

      <div class="text-container">
        <h5 class>
          <b>{{ movie.title}}</b>
        </h5>
        <p class="col s7">Datum: {{ screening.startTime.toDate().toLocaleDateString() }}</p>
        <p class="col s7">Tid: {{ screening.startTime.toDate().getHours() }}:00</p>
        <p class="col s7">Pris: {{ bookingObject.totalTicketPrice }} kronor</p>
        <p class="col s7 bookingnr">
          <b>Bokningsnummer: {{ bookingObject.id.slice(-6) }}</b>
        </p>
      </div>
    </div>
    <i
      v-if="!isHidden"
      @click="toggleInfo"
      class="material-icons medium col s2 black-text test"
    >navigate_next</i>

    <div
      class="inner-container white black-text test left-align"
      v-if="isHidden"
      @click="toggleInfo"
    >
      <h5 class="col s9">Biljetter</h5>
      <span
        class="col s9"
        v-if="bookingObject.adultTickets"
      >{{ bookingObject.adultTickets}}x vuxenbiljett</span>
      <span
        class="col s9"
        v-if="bookingObject.childTickets"
      >{{ bookingObject.childTickets }}x barnbiljett</span>
      <span
        class="col s9"
        v-if="bookingObject.seniorTickets"
      >{{bookingObject.seniorTickets }}x pensionär</span>
      <h5 class="col s9">Platser</h5>
      <span
        class="col s9"
        v-for="(seat, id) in bookingObject.seats"
        :key="id"
      >rad {{ seat.y + 1 }}, plats {{ seat.x}}</span>
    </div>
    <i
      v-if="isHidden"
      @click="toggleInfo"
      class="material-icons medium col s2 black-text left"
    >navigate_before</i>
  </div>
</template>

<script>
import { filterItemFromList } from "./utils/logicUtils.js";

export default {
  data() {
    return {
      isHidden: false
    };
  },
  props: {
    bookingObject: {
      type: Object,
      required: true
    }
  },
  methods: {
    toggleInfo() {
      this.isHidden = !this.isHidden;
    }
  },
  computed: {
    screening() {
      return filterItemFromList(
        this.$store.state.screenings,
        this.bookingObject.screeningId
      );
    },
    movie() {
      return filterItemFromList(
        this.$store.state.movies,
        this.screening.movieId
      );
    },
    auditorium() {
      return filterItemFromList(
        this.$store.state.auditoriums,
        this.screening.auditoriumId
      );
    }
  }
};
</script>
<style scoped>
.my-bookings {
  border-radius: 15px;
  padding-right: 0 !important;
}
.text-container p {
  padding-right: 0 !important;
  padding-left: 3% !important;
  margin: 0 !important;
}
.outer-container {
  border-radius: 15px;
}
.image {
  height: 100% !important;
  min-height: 100px;
  display: block;
}
.hidden-div {
  width: 0px;
}

.inner-container {
  border-radius: 15px;
}
.image-container {
  margin: 0 !important;
  padding: 0 !important;
  height: calc(100vh / 4);
  width: auto !important;
}
.row {
  padding: 0 !important;
  margin: 1rem !important;
}
h5 {
  margin-top: 1% !important;
  margin-bottom: 0.5% !important;
  padding-left: 3% !important;
}
h5 b {
  padding-left: 3% !important;
}
.test {
  padding-left: 0 !important;
}

.bookingnr {
  margin-bottom: 0px;
  padding-bottom: 2%;
}
@media screen and (max-width: 520px) {
  .my-bookings {
    font-size: 0.8rem;
  }
  h5 {
    font-size: 1.1rem;
    margin-bottom: 2% !important;
  }
  .image-container {
    height: calc(100vh / 5);
  }
}
@media screen and (max-width: 376px) {
  .my-bookings {
    font-size: 0.7rem;
  }
  h5 {
    font-size: 0.9rem;
    margin-bottom: 3% !important;
  }
  .image-container {
    height: calc(100vh / 5);
  }
}
</style>