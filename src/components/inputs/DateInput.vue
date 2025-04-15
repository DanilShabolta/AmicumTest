<template>
  <div class="date-input">
    <div class="calendar-wrapper">
      <date-picker-custom
        v-model="internalValue"
        :error="error"
        @clear="clearDate"
        @focus="handleFocus"
        @input-text="updateInputText"
        ref="customPicker"
      />
      <span v-if="inputText" class="clear-icon" @click="clearDate">×</span>
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
      inputText: "",
    };
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
      this.$refs.customPicker?.clearInput();
    },
    handleFocus(event) {
      event.preventDefault();
    },
    updateInputText(text) {
      this.inputText = text;
    },
  },
};
</script>
