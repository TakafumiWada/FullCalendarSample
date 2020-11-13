<template>
  <div id="app">
    <div>
      <input name="date" type="date" v-model="startDate" style="height:20px"/>
      <input type="time" v-model="startTime">
    <input name="date" type="date" v-model="endDate" style="height:20px"/>
      <input type="time" v-model="endTime">

      <input type="text" v-model="addText" style="height:20px">
      <button v-on:click="addEvent">追加</button>
    </div>
    <div>
      <input type="checkbox" v-model="calendarOptions.weekends">weekends
    </div>
    <FullCalendar :options="calendarOptions" />
  </div>
</template>

<script>
import FullCalendar from '@fullcalendar/vue'
import dayGridPlugin from '@fullcalendar/daygrid'
import timeGridPlugin from '@fullcalendar/timegrid'
import interactionPlugin from '@fullcalendar/interaction'

export default {
  name: 'App',
  components: {
    FullCalendar // make the <FullCalendar> tag available
  },
  data() {
    return {
      calendarOptions: {
        plugins: [ dayGridPlugin,timeGridPlugin, interactionPlugin ],
        events: [
          { title: 'event 1', date: '2020-11-01' },
          { title: 'event 2', date: '2020-11-02' },
          { title: 'event 3', start: '2020-11-02T10:00', end: '2020-11-02T11:00'},

        ],
        headerToolbar: {
          left: 'prev,next today',
          center: 'title',
          right: 'dayGridMonth,timeGridWeek,timeGridDay'
        },
        initialView: 'dayGridMonth',
        dateClick: this.handleDateClick,

        editable: true,
        selectable: true,
        selectMirror: true,
        //initialEvents: INITIAL_EVENTS,
        weekends: true
      },
      addText:"",
      startDate:"",
      endDate:"",
      startTime:"",
      endTime:""
    }
  },
  created(){
    this.nowTimeSetting()
   
  },
   methods: {
     nowTimeSetting(){
        var today = new Date();
        today.setDate(today.getDate());
        var yyyy = today.getFullYear();
        var mm = ("0"+(today.getMonth()+1)).slice(-2);
        var dd = ("0"+today.getDate()).slice(-2);
        this.startDate=yyyy+'-'+mm+'-'+dd;
        this.endDate=yyyy+'-'+mm+'-'+dd;
        var dt = new Date();
        var h  = dt.getHours();
        this.startTime=h+":00"
        this.endTime=h+1+":00"
     },
    handleDateClick: function(arg) {
      //alert('date click! ' + arg.dateStr)
      this.startDate=arg.dateStr
    },
    toggleWeekends: function() {
      this.calendarOptions.weekends = !this.calendarOptions.weekends // toggle the boolean!
    },
    addEvent:function(){
      this.calendarOptions.events.push({title:this.addText,start:this.startDate+"T"+this.startTime,end:this.endDate+"T"+this.endTime})
      this.addText=""
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
