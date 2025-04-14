<template>
  <div class="date-input">
    <div class="calendar-wrapper">
      <date-picker-custom
        v-model="internalValue"
        :error="error"
        @clear="clearDate"
        ref="customPicker"
        @focus="handleFocus"
      />
      <span v-if="internalValue" class="clear-icon" @click="clearDate">×</span>
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
    },
    handleFocus(event) {
      event.preventDefault();
    },
  },
};
</script>

<style scoped>
.calendar-wrapper {
  position: relative;
  display: flex;
  align-items: center;
  width: 100%;
}

.clear-icon {
  position: absolute;
  right: 30px;
  font-size: 18px;
  color: #999;
  cursor: pointer;
  z-index: 2;
}

.calendar-icon {
  position: absolute;
  right: 8px;
  font-size: 16px;
  cursor: pointer;
  z-index: 2;
}
</style>
