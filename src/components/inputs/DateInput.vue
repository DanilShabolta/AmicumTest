<template>
  <div class="date-input">
    <div class="calendar-wrapper">
      <date-picker-custom
        v-model="internalValue"
        :error="error"
        @clear="clearDate"
        ref="customPicker"
        @focus="handleFocus"
        @input="handleInput"
      />
      <span v-if="showClearIcon" class="clear-icon" @click="clearDate">×</span>
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
    };
  },
  computed: {
    showClearIcon() {
      return this.rawText.length > 0 || !!this.internalValue;
    },
  },
  watch: {
    value(newVal) {
      this.internalValue = newVal;
    },
    internalValue(newVal) {
      this.$emit("input", newVal);
    },
  },
  methods: {
    toggleCalendar() {
      this.$refs.customPicker?.toggle();
    },
    clearDate() {
      this.internalValue = null;
      this.rawText = "";
      this.$refs.customPicker?.clearInput();
    },
    handleFocus(event) {
      event.preventDefault();
    },
    handleInput() {
      this.rawText = this.$refs.customPicker?.getMaskedValue() || "";
    },
  },
};
</script>
