<template>
  <div>
    <img alt="Vue logo" src="./assets/reminder.jpg" />
    <Reminder msg="Event Reminder" />
  </div>
</template>

<script>
import Reminder from "./components/Reminder.vue";
export default {
  name: "App",
  components: {
    Reminder,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 10px;
  margin-bottom: 60px;
  font-size:30px;
  margin-left:45px;
}
img{
  height: 150px;
}
</style>
