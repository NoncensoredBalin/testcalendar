<template>
  <div id="app">
    <h1>Календарь</h1>
    <Calendar :initial-date="initialDate" @date-selected="onDateSelected" />
    <div v-if="selectedDate" style="margin-top: 20px;">
      Выбранная дата: {{ selectedDate }}
    </div>
  </div>
</template>

<script>
import Calendar from './components/Calendar.vue'

export default {
  name: 'App',
  components: {
    Calendar
  },
  data() {
    return {
      initialDate: '2025-10-08',
      selectedDate: null
    }
  },
  methods: {
    onDateSelected(date) {
      this.selectedDate = date
      console.log('Выбрана дата:', date)
    }
  }
}
</script>

<style>
#app {
  font-family: Arial, sans-serif;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 20px;
}

h1 {
  margin-bottom: 20px;
}
</style>

