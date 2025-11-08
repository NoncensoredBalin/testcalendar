# Vue Calendar Component

Простой компонент календаря на Vue.js 2.

## Возможности

- Переключение месяцев (кнопки влево/вправо)
- Выбор даты кликом
- Событие `date-selected` при выборе даты
- Поддержка начальной даты через prop `initial-date`
- Смена языка (русский/английский)

## Использование

```vue
<template>
  <Calendar 
    :initial-date="'2024-03-15'" 
    @date-selected="handleDateSelect" 
  />
</template>

<script>
import Calendar from './components/Calendar.vue'

export default {
  components: {
    Calendar
  },
  methods: {
    handleDateSelect(date) {
      console.log('Выбрана дата:', date) // формат: "2024-03-15"
    }
  }
}
</script>
```

## Props

- `initial-date` (String, опционально) - начальная дата в формате "год-месяц-день" (например, "2024-03-15"). Если не указана, используется текущая дата.

## Events

- `date-selected` - вызывается при клике на день. Передает строку с датой в формате "год-месяц-день".

## Установка

```bash
npm install
```

## Запуск

```bash
npm run serve
```

