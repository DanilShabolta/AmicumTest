<template>
  <div class="date-input">
    <div
      class="calendar-wrapper"
      :class="{
        'has-value': rawText || internalValue,
        'has-error': error,
      }"
    >
      <date-time-picker-custom
        v-model="internalValue"
        :error="error"
        @clear="clearDate"
        @raw-input="rawText = $event"
        ref="customPicker"
        :clear-trigger="clearTrigger"
      />
      <span
        v-if="rawText || internalValue"
        class="clear-icon"
        @click="clearDate"
        >×</span
      >
      <i
        class="vue2-datepicker__icon"
        @click="toggleCalendar"
        role="button"
      ></i>
    </div>
  </div>
</template>

<script>
import DateTimePickerCustom from "../customPickers/DateTimePickerCustom.vue";

export default {
  name: "DateTimeInput",
  components: { DateTimePickerCustom },
  props: {
    value: { type: Date, default: null },
    error: { type: Boolean, default: false },
  },
  data() {
    return {
      internalValue: this.value,
      rawText: "",
      clearTrigger: false,
    };
  },
  watch: {
    value(newVal) {
      this.internalValue = newVal;
    },
    internalValue(newVal) {
      this.$emit("input", newVal);
      this.rawText = newVal ? this.formatDate(newVal) : "";
    },
  },
  methods: {
    toggleCalendar() {
      this.$refs.customPicker?.toggle();
    },
    clearDate() {
      this.internalValue = null;
      this.rawText = "";
      this.clearTrigger = !this.clearTrigger;
    },
    formatDate(date) {
      if (!date) return "";
      const pad = (n) => String(n).padStart(2, "0");
      return `${pad(date.getDate())}.${pad(
        date.getMonth() + 1
      )}.${date.getFullYear()} ${pad(date.getHours())}:${pad(
        date.getMinutes()
      )}`;
    },
  },
};
</script>
