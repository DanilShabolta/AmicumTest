<template>
  <div id="app">
    <ThemeSwitcher />

    <DateInput v-model="selectedDate" />
    <h2></h2>

    <DateRangeInput v-model="selectedRange" />

    <h2></h2>
    <DateTimeInput v-model="dateTime" />
  </div>
</template>

<script>
import DateInput from "./components/inputs/DateInput.vue";
import DateRangeInput from "./components/inputs/DateRangeInput.vue";
import DateTimeInput from "./components/inputs/DateTimeInput.vue";
import ThemeSwitcher from "./components/ThemeSwitcher.vue";
import "./assets/styles/main.css";

export default {
  name: "App",
  components: {
    DateInput,
    DateRangeInput,
    DateTimeInput,
    ThemeSwitcher,
  },
  data() {
    return {
      selectedDate: null,
      selectedRange: [null, null],
      dateTime: null,
    };
  },
};
</script>
