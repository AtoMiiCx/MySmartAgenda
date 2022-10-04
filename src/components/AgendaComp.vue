<script setup>
import { reactive, ref } from "vue"; // Here we implement the array of plugin
import "@fullcalendar/core/vdom"; // Here we implement the style of the calendar
import Fullcalendar from "@fullcalendar/vue3";
import DayGridPlugin from "@fullcalendar/daygrid";
import TimeGridPlugin from "@fullcalendar/timegrid";
import InteractionPlugin from "@fullcalendar/interaction";
import ListPlugin from "@fullcalendar/list";

//id sera le nombre de case selectioné par l'utilisateur
const id = ref(10);

//Here we create an array of plugin inorder to implement it
const options = reactive({
  plugins: [DayGridPlugin, TimeGridPlugin, InteractionPlugin, ListPlugin],

  initalView: "dayGridMonth",
  // Here we can reorganize the bouton of the headr of the calendar
  headerToolbar: {
    left: "prev,today,next",
    center: "title",
    right: "dayGridMonth,dayGridWeek,listDay",
  },
  editable: true,
  selectable: true,
  weekends: true, // here we can choose to keep or not the weekends
  //selectionner or clik on case on the calendar
  select: (arg) => {
    id.value = id.value + 1; //number id grow at each time we call this fonction

    const cal = arg.view.calendar; // here we can know the number of case select to have info on it
    cal.unselect(); //here we unselect the data that we have took thanks to the argument (cal)
    //and thent what we have select becom an event with the folowing propr
    cal.addEvent({
      id: `${id.value}`,
      title: `New event ${id.value}`,
      start: arg.start,
      end: arg.end,
      allDay: true, // the event is all the day
    });
  },
  //to be able to clic on the envent bar we add event click option
  eventClick: (arg) => {
    console.log(arg.event.title); // we can get the data from the argument
  },
});
</script>

<template>
  <div class="container w-auto">
    <Fullcalendar v-bind:options="options" />
  </div>
</template>

<style scoped></style>
