<template>
  <div id="app">
    <ThemeSwitcher />

    <DateInput v-model="selectedDate" />

    <DateRangeInput v-model="selectedRange" />

    <h2>Демо: Дата и время</h2>
    <DateTimeInput v-model="dateTime" />
    <p>Дата и время: {{ dateTime }}</p>

    <h2>Демо: Период с датой и временем</h2>
    <DateTimeRangeInput v-model="dateTimeRange" />
    <p>Период: {{ dateTimeRange }}</p>
  </div>
</template>

<script>
import DateInput from "./components/inputs/DateInput.vue";
import DateRangeInput from "./components/inputs/DateRangeInput.vue";
import DateTimeInput from "./components/inputs/DateTimeInput.vue";
import DateTimeRangeInput from "./components/inputs/DateTimeRangeInput.vue";
import ThemeSwitcher from "./components/ThemeSwitcher.vue";
import "./assets/styles/main.css";

export default {
  name: "App",
  components: {
    DateInput,
    DateRangeInput,
    DateTimeInput,
    DateTimeRangeInput,
    ThemeSwitcher,
  },
  data() {
    return {
      selectedDate: null,
      selectedRange: [null, null],
      dateTime: null,
      dateTimeRange: [null, null],
    };
  },
};
</script>
