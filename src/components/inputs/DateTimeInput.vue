<template>
  <div class="date-time-input">
    <div class="input-wrapper">
      <input
        class="manual-input"
        v-model="dateTimeString"
        placeholder="ДД.ММ.ГГГГ ЧЧ:ММ"
        @blur="parseDateTime"
      />
      <date-time-picker-custom v-model="internalValue" />
    </div>
  </div>
</template>

<script>
import DateTimePickerCustom from "../customPickers/DateTimePickerCustom.vue";

export default {
  name: "DateTimeInput",
  components: { DateTimePickerCustom },
  props: {
    value: Date,
    label: {
      type: String,
      default: "Дата и время",
    },
    error: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      internalValue: this.value || null,
      dateTimeString: this.value ? this.formatDateTime(this.value) : "",
    };
  },
  watch: {
    value(newVal) {
      this.internalValue = newVal;
      this.dateTimeString = this.formatDateTime(newVal);
    },
    internalValue(newVal) {
      this.dateTimeString = this.formatDateTime(newVal);
      this.$emit("input", newVal);
    },
  },
  methods: {
    formatDateTime(date) {
      if (!date) return "";
      const d = new Date(date);
      const day = String(d.getDate()).padStart(2, "0");
      const month = String(d.getMonth() + 1).padStart(2, "0");
      const year = d.getFullYear();
      const hours = String(d.getHours()).padStart(2, "0");
      const minutes = String(d.getMinutes()).padStart(2, "0");
      return `${day}.${month}.${year} ${hours}:${minutes}`;
    },
    parseDateTime() {
      const parts = this.dateTimeString.split(" ");
      if (parts.length !== 2) return;
      const [datePart, timePart] = parts;
      const [day, month, year] = datePart.split(".");
      const [hours, minutes] = timePart.split(":");
      const parsed = new Date(`${year}-${month}-${day}T${hours}:${minutes}`);
      if (!isNaN(parsed)) {
        this.internalValue = parsed;
        this.$emit("input", parsed);
      }
    },
  },
};
</script>
