<template>
  <div class="mt-8">
    <h2>{{ dateMessage }}</h2>
    <textarea
      class="notepad"
      v-model="text"
      @keyup="setLocalStorage"
    >
    </textarea>
  </div>
</template>
<script>
export default {
  name: "Notes",
  data() {
    return {
      text: "",
      dark: false
    };
  },
  methods: {
    setLocalStorage() {
      window.localStorage.setItem("userText", this.text);
    }
  },
  computed: {
    months() {
      return [
        "January",
        "February",
        "March",
        "April",
        "May",
        "June",
        "July",
        "August",
        "September",
        "October",
        "November",
        "December"
      ];
    },
    weekDays() {
      return [
        "Sunday",
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday",
        "Saturday"
      ];
    },
    getMonthNumber() {
      return new Date().getMonth();
    },
    getDate() {
      return new Date().getDate();
    },
    getDay() {
      const day = new Date().getDay();
      return this.weekDays[day];
    },
    getHour() {
      return new Date().getHours();
    },
    getMinutes() {
      // get minutes
      const minutes = new Date().getMinutes();
      // if 0-9 add 0 before to make e.g 01
      // else return whole minutes eg 25
      if (minutes.toString().length === 1) return `0${minutes}`;
      return minutes;
    },
    checkAmPM() {
      if (this.getHour < 12) return "am";
      return "pm";
    },
    checkGreeting() {
      if (this.getHour < 12) return "Good Morning";
    },
    ordinalIndicator() {
      // get st,nd,rd
      const nth = function(d) {
        if (d > 3 && d < 21) return "th";
        // d = 1 / 10 remaining = 0.1 case = 1
        // d = 22 / 10 remaining = 0.2 case = 2
        switch (d % 10) {
          case 1:
            return "st";
          case 2:
            return "nd";
          case 3:
            return "rd";
          default:
            return "th";
        }
      };

      return nth(this.getDate);
    },
    dateMessage() {
      return `${this.checkGreeting} it is  ${this.getHour}:${this.getMinutes} ${
        this.checkAmPM
      },
    ${this.getDay} ${this.getDate}${this.ordinalIndicator} ${
        this.months[this.getMonthNumber]
      }`;
    }
  },
  mounted() {
    // if note has been stored previously use this from local storage
    if (window.localStorage.getItem("userText")) {
      this.text = window.localStorage.getItem("userText");
    }
  }
};
</script>
<style>
.notepad {
  width: 90%;
  border: 1px solid grey;
  height: 90vh;
  padding: 20px;
  appearance: none;
  background: transparent;
  border: none;
  flex: 1 1 100%;
  font-family: var(--fontStack) !important;
  margin-bottom: 1.5rem;
  outline: none;
  padding-top: 1.5rem;
  resize: none;
  font-size: 18px;
}
</style>
