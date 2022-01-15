<template>
  <div>
    <h1>{{ msg }}</h1>
    <form  @submit="checkForm">
      <Datepicker
        id="eventDate"
        v-model="eventDate"
        placeholder="Choose Event Date and Time"
      ></Datepicker>
      <h2>
        <textarea
          id="eventDetails"
          v-model="eventDetails"
          placeholder="Add Event Details"
        ></textarea>
      </h2>  
      <h3>
        <select v-model="interval" id="interval">
        <option disabled value="">Select Interval</option>
        <option value="1">1 Minute</option>
        <option value="5">5 Minutes</option>
        <option value="15">15 Minutes</option>
        <option value="30">30 Minutes</option>
        <option value="60">1 Hour</option>
        <option value="360">6 Hours </option>
        <option value="720">12 Hours</option>
        <option value="1440">1 Day</option>
        </select>
      </h3>
      <Datepicker
        id="expirationDate"
        v-model="expirationDate"
        placeholder="Choose Event's Expiration"
      ></Datepicker>
      <input v-model="name" id="name" placeholder="First Name" />
      <input v-model="surname" id="surname" placeholder="Last Name" />
      <h4>
        <button type="submit" @click="submit">Submit</button>
      </h4>  
      <p v-if="errors.length">
        <b>Please correct the following error(s):</b>
        <ul>
        <li v-for="error in errors" :key=error>{{ error }}</li>
        </ul>
      </p>
    </form>
  </div>
</template>

<script>
import Datepicker from "vue3-date-time-picker";
import "vue3-date-time-picker/dist/main.css";
export default {
  name: "Reminder",
  props: {
    msg: String,
  },
  components: { Datepicker },
  data() {
    return {
      errors: [],
      events: [],
      eventDate: "",
      expirationDate: "",
      eventDetails: "",
      name: "",
      surname: "",
      interval: "",
    };
  },
  //gia kathe event ginontai elegxoi an uparxei interval, exipiration date
  //kai analoga emfanizontai ta alert me ta stoixeia pou exei sumplhrwsei o xrhsths
  created() {
    setInterval(() => {
      this.events.forEach((event,index) => {
        var currentDate = new Date();
        if (event.eventDate <= currentDate) {
          event.timesTriggered += 1;
          alert(
            "Event details: " +
              event.eventDetails +
              "\nName: " +
              event.name +
              "\nLast Name: " +
              event.surname +
              "\nEvent Date: " +
              event.eventDate +
              "\n Interval: " +
              event.interval +
              " minutes" +
              "\n Notifications sent: " +
              event.timesTriggered
          );
          var passedTime = event.eventDate.getTime() + event.interval * 60000
          if (event.interval > 0 && ( event.expirationDate == "" || passedTime <= event.expirationDate)) {
            event.eventDate.setTime(passedTime);
          }else{
            this.events.splice(index,1)
          }
        }
      });
    }, 60000);
  },
  methods: {
    //elegxos gia or8h sumplirwosi formas
    checkForm: function (e) {
      this.errors = [];
      if (!this.eventDate) {
        this.errors.push("Event Date required.");
      }
      if (!this.eventDetails) {
        this.errors.push("Event Details required.");
      }
      if (this.expirationDate && this.expirationDate < this.eventDate)
        this.errors.push(
          "Expiration Date must be a later date than Event Date."
        );
      e.preventDefault();
      if (this.errors.length == 0) {
        //eisagwgh stoixeiwn sto object Events
        this.events.push({
          eventDate: this.eventDate,
          expirationDate: this.expirationDate,
          eventDetails: this.eventDetails,
          name: this.name,
          surname: this.surname,
          interval: this.interval,
          timesTriggered: 0,
        });
        //Katharismos formas
        this.eventDate = "";
        this.name = "";
        this.surname = "";
        this.eventDetails = "";
        this.expirationDate = "";
        this.interval = "";
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#eventDate {
  margin-bottom: 25px;
  margin-left: 500px;
  width: 800px;
  border-block-end-style: double;
  border-top-style: double;
  border-color: chocolate;
}
#expirationDate {
  margin-bottom: 25px;
  margin-left: 500px;
  width: 800px;
  border-block-end-style: double;
  border-top-style: double;
  border-color: chocolate;
}
#eventDetails {
  text-align: center;
  text-size-adjust: inherit;
  font-size: 50px;
  width: 666px;
  height: 200px;
  border-style: dashed;
}
h2 {
  margin-bottom: 5px;
  align-items: center;
}
h3 {
  margin-bottom: 15px;
  align-items: center;
}
#interval {
  width: 250px;
}
#name {
  width: 170px;
  height: 25px;
  margin-right: 10px;
  border-block-end-style: solid;
  border-color: chocolate;
}
#surname {
  width: 170px;
  height: 25px;
  margin-right: 10px;
  border-block-end-style: solid;
  border-color: chocolate;
}
h4 {
  align-self: center;
}
button {
  width: 100px;
  height: 30px;
  background-color: rgb(202, 142, 13);
  font-size: 18px;
}
p {
  font-size: medium;
  color: red;
}
</style>
