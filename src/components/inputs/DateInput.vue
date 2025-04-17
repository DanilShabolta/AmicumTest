<template>
  <div class="date-input">
    <div
      class="calendar-wrapper"
      :class="{
        'has-value': rawText || internalValue,
        'has-error': error,
      }"
    >
      <date-picker-custom
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
import DatePickerCustom from "../customPickers/DatePickerCustom.vue";

export default {
  name: "DateInput",
  components: { DatePickerCustom },
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
      return date
        ? `${String(date.getDate()).padStart(2, "0")}.${String(
            date.getMonth() + 1
          ).padStart(2, "0")}.${date.getFullYear()}`
        : "";
    },
  },
};
</script>
