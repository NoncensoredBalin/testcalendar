<template>
  <div class="calendar">
    <div class="calendar-header">
      <button @click="prevMonth" class="nav-btn">‹</button>
      <div class="month-year">{{ currentMonthName }} {{ currentYear }}</div>
      <button @click="nextMonth" class="nav-btn">›</button>
    </div>
    
    <div class="language-selector">
      <select v-model="currentLang">
        <option value="ru">Русский</option>
        <option value="en">English</option>
      </select>
    </div>

    <div class="weekdays">
      <div v-for="day in weekdays" :key="day" class="weekday">{{ day }}</div>
    </div>

    <div class="days">
      <div
        v-for="(day, index) in days"
        :key="index"
        :class="['day', { 'other-month': day.otherMonth, 'selected': day.selected, 'today': day.today }]"
        @click="selectDate(day.date)"
      >
        {{ day.number }}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Calendar',
  props: {
    initialDate: {
      type: String,
      default: null
    }
  },
  data() {
    return {
      currentDate: null,
      selectedDate: null,
      currentLang: 'ru',
      languages: {
        ru: {
          months: ['Январь', 'Февраль', 'Март', 'Апрель', 'Май', 'Июнь', 'Июль', 'Август', 'Сентябрь', 'Октябрь', 'Ноябрь', 'Декабрь'],
          weekdays: ['Пн', 'Вт', 'Ср', 'Чт', 'Пт', 'Сб', 'Вс']
        },
        en: {
          months: ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'],
          weekdays: ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
        }
      }
    }
  },
  computed: {
    currentYear() {
      return this.currentDate.getFullYear()
    },
    currentMonth() {
      return this.currentDate.getMonth()
    },
    currentMonthName() {
      return this.languages[this.currentLang].months[this.currentMonth]
    },
    weekdays() {
      return this.languages[this.currentLang].weekdays
    },
    days() {
      const year = this.currentYear
      const month = this.currentMonth
      const firstDay = new Date(year, month, 1)
      const lastDay = new Date(year, month + 1, 0)
      const daysInMonth = lastDay.getDate()
      let startDay = firstDay.getDay()
      if (startDay === 0) startDay = 7
      startDay = startDay - 1
      
      const result = []
      const today = new Date()
      const selected = this.selectedDateObj
      
      const prevMonthDays = new Date(year, month, 0).getDate()
      for (let i = startDay; i > 0; i--) {
        result.push({
          number: prevMonthDays - i + 1,
          date: new Date(year, month - 1, prevMonthDays - i + 1),
          otherMonth: true
        })
      }
      
      for (let i = 1; i <= daysInMonth; i++) {
        const d = new Date(year, month, i)
        result.push({
          number: i,
          date: d,
          otherMonth: false,
          today: d.toDateString() === today.toDateString(),
          selected: selected && d.toDateString() === selected.toDateString()
        })
      }
      
      let needDays = 42 - result.length
      for (let i = 1; i <= needDays; i++) {
        result.push({
          number: i,
          date: new Date(year, month + 1, i),
          otherMonth: true
        })
      }
      
      return result
    },
    selectedDateObj() {
      return this.selectedDate ? new Date(this.selectedDate) : null
    }
  },
  created() {
    if (this.initialDate) {
      const parts = this.initialDate.split('-')
      const date = new Date(parts[0], parts[1] - 1, parts[2])
      this.currentDate = date
      this.selectedDate = date
    } else {
      this.currentDate = new Date()
    }
  },
  methods: {
    prevMonth() {
      this.currentDate = new Date(this.currentYear, this.currentMonth - 1, 1)
    },
    nextMonth() {
      this.currentDate = new Date(this.currentYear, this.currentMonth + 1, 1)
    },
    selectDate(date) {
      this.selectedDate = date
      this.$emit('date-selected', this.formatDate(date))
    },
    formatDate(date) {
      let year = date.getFullYear()
      let month = date.getMonth() + 1
      let day = date.getDate()
      if (month < 10) month = '0' + month
      else month = '' + month
      if (day < 10) day = '0' + day
      else day = '' + day
      return year + '-' + month + '-' + day
    }
  }
}
</script>

<style scoped>
.calendar {
  width: 300px;
  border: 1px solid #ddd;
  border-radius: 4px;
  padding: 10px;
  font-family: Arial, sans-serif;
}

.calendar-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}

.month-year {
  font-weight: bold;
  font-size: 16px;
}

.nav-btn {
  background: none;
  border: 1px solid #ddd;
  cursor: pointer;
  padding: 5px 10px;
  font-size: 18px;
  border-radius: 3px;
}

.nav-btn:hover {
  background: #f0f0f0;
}

.language-selector {
  margin-bottom: 10px;
}

.language-selector select {
  padding: 5px;
  border: 1px solid #ddd;
  border-radius: 3px;
}

.weekdays {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  gap: 2px;
  margin-bottom: 5px;
}

.weekday {
  text-align: center;
  font-weight: bold;
  font-size: 12px;
  padding: 5px;
  color: #666;
}

.days {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  gap: 2px;
}

.day {
  text-align: center;
  padding: 8px;
  cursor: pointer;
  border-radius: 3px;
}

.day:hover {
  background: #e8f4f8;
}

.day.other-month {
  color: #ccc;
}

.day.today {
  background: #e8f4f8;
  font-weight: bold;
}

.day.selected {
  background: #4a90e2;
  color: white;
}

.day.selected.today {
  background: #4a90e2;
  color: white;
}
</style>

