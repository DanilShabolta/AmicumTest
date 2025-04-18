<template>
  <div class="date-input">
    <div
      class="calendar-wrapper"
      :class="{
        'has-value':
          rawText.start ||
          rawText.end ||
          internalValue.start ||
          internalValue.end,
        'has-error': error,
      }"
    >
      <date-range-picker-custom
        :value="internalValue"
        :error="error"
        @input="onPickerInput"
        @clear="clearDate"
        @raw-input="updateRawText"
        ref="customPicker"
        :clear-trigger="clearTrigger"
      />
      <span
        v-if="
          internalValue.start ||
          internalValue.end ||
          rawText.start ||
          rawText.end
        "
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
import DateRangePickerCustom from "../customPickers/DateRangePickerCustom.vue";

export default {
  name: "DateRangeInput",
  components: { DateRangePickerCustom },
  props: {
    value: { type: Object, default: () => ({ start: null, end: null }) },
    error: { type: Boolean, default: false },
  },
  data() {
    return {
      internalValue: this.value
        ? { ...this.value }
        : { start: null, end: null },
      rawText: { start: "", end: "" },
      clearTrigger: false,
    };
  },
  watch: {
    value(newVal) {
      this.internalValue = { ...newVal };
    },
    watch: {
      internalValue(newVal) {
        if (Array.isArray(newVal)) {
          this.$emit("input", { start: newVal[0], end: newVal[1] });
        } else {
          this.$emit("input", newVal);
        }
        this.rawText = {
          start: newVal.start ? this.formatDate(newVal.start) : "",
          end: newVal.end ? this.formatDate(newVal.end) : "",
        };
      },
    },
  },
  methods: {
    toggleCalendar() {
      this.$refs.customPicker?.toggle();
    },
    clearDate() {
      this.internalValue = { start: null, end: null };
      this.rawText = { start: "", end: "" };
    },
    formatDate(date) {
      return date
        ? `${String(date.getDate()).padStart(2, "0")}.${String(
            date.getMonth() + 1
          ).padStart(2, "0")}.${date.getFullYear()}`
        : "";
    },
    updateRawText(val) {
      this.rawText = val;
    },
    onPickerInput(newVal) {
      this.internalValue = newVal;
      this.$emit("input", newVal);
    },
  },
};
</script>
