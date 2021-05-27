<template>
  <div class="app">

    <div class="header">
      <h1> Vue Calender</h1>
    </div>
    <section class="currentMonth">
      <h2>{{ currentMonthName }}</h2>
      <h2>{{ currentYear }}</h2>
    </section>

    <section class="section">
      <p v-for="day in days" :key="day">{{ day }}</p>
    </section>
    
    <section class="section1">
      <p v-for="num in startDay()"  :key="num"></p>
      <p v-for="num in daysInMonth(currentYear,currentMonth)" :style="currentDateClass(num)" :key="num">{{num}}</p>
    </section>

    <section class="btnSection">
      <button @click="prev">prev</button>
      <button @click="next">Next</button>
    </section>
  </div>
</template>

<script>
import { computed, reactive, ref } from 'vue'

export default {
  name: 'Calender',
  props: { },

  setup() {
    let currentMonth = ref(new Date().getMonth())
    const currentYear = ref(new Date().getFullYear())
    const currentDate = new Date().getUTCDate()
    const changeColor = {
      color : 'blue',
      fontWeight : "900"
    }
   

    const days = reactive([
      'Sun', 
      'Mon',
      'Tue',
      'Wed',
      'Thu',
      'Fri',
      'Sat'
    ])

    function daysInMonth (year, month) {
        return new Date(year, month + 1, 0).getDate()
    }

    function startDay() {
      return new Date(currentYear.value, currentMonth.value , 1).getDay()
    }

    function next() {
      if(currentMonth.value === 11) {
        currentMonth.value = 0
        currentYear.value ++
      }else {
        currentMonth.value++
      }
    }
    function prev() {
      if(currentMonth.value === 0){
        currentMonth.value = 11
        currentYear.value--
      }else {
        currentMonth.value--
      }
    }

    const currentMonthName = computed(() => {
      return new Date(
        currentYear.value,
        currentMonth.value,
      ).toLocaleString('default', {month: 'long'})
    })

    function currentDateClass(num) {
      const calenderFullDate = new Date(
        currentYear.value, 
        currentMonth.value,
        num
        ).toDateString()
        const currentFullDate = new Date().toDateString();
        return calenderFullDate === currentFullDate ? changeColor : " ";
    }

    return {
      days,
      currentMonth,
      currentYear,
      daysInMonth,
      currentMonthName,
      startDay,
      next,
      prev,
      currentDate,
      changeColor,
      currentDateClass
    }
  }
}
</script>

<style scoped>
.app{
  /* border: 1px solid red; */
  width: fit-content;
  margin: auto;
}
.header {
  width: fit-content;
  margin: auto;
  
}

.section   {
  display: flex;
  /* border: 1px solid red; */
  text-align: center;
  width: fit-content;
  margin: auto;
}

.section p {
  width: 5rem;
  height: 1rem;
}

.section1 p {
  width: 14.28%;
}

.section1 {
  display: flex;
  text-align: center;
  flex-wrap: wrap;
}

.currentMonth {
  display: flex;
  justify-content: space-between;
}

.currentMonth h2{
  font-weight: 900;
}

.btnSection {
  display: flex;
  justify-content: space-between;
}

.btnSection >  * {
  border:  1px solid black;
  border-radius: 2px;
  padding: 2px 1.5em;
  cursor: pointer;
}
</style>
