<template>
  <div class="date-range-input">
    <div
      class="calendar-wrapper"
      :class="{
        'has-value': rawText || (internalRange[0] && internalRange[1]),
        'has-error': error,
      }"
    >
      <date-range-picker-custom
        v-model="internalRange"
        :error="error"
        @clear="clearRange"
        @raw-input="rawText = $event"
        ref="customPicker"
        :clear-trigger="clearTrigger"
      />
      <span
        v-if="rawText || (internalRange[0] && internalRange[1])"
        class="clear-icon"
        @click="clearRange"
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
    value: {
      type: Array,
      default: () => [null, null],
    },
    error: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      internalRange: [...this.value],
      rawText: "",
      clearTrigger: false,
    };
  },
  watch: {
    value(newVal) {
      this.internalRange = [...newVal];
    },
    internalRange(newVal) {
      this.$emit("input", newVal);
      this.rawText = this.formatRange(newVal);
    },
  },
  methods: {
    toggleCalendar() {
      this.$refs.customPicker?.toggle();
    },
    clearRange() {
      this.internalRange = [null, null];
      this.rawText = "";
      this.clearTrigger = !this.clearTrigger;
    },
    formatRange([start, end]) {
      if (!start || !end) return "";
      const formatDate = (d) =>
        `${String(d.getDate()).padStart(2, "0")}.${String(
          d.getMonth() + 1
        ).padStart(2, "0")}.${d.getFullYear()}`;
      return `${formatDate(start)} - ${formatDate(end)}`;
    },
  },
};
</script>
