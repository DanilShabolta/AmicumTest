<template>
  <div class="date-range-input">
    <input
      class="manual-input"
      :value="formattedValue"
      placeholder="Введите период"
    />
    <div class="calendar-wrapper">
      <date-range-picker-custom v-model="internalValue" />
    </div>
  </div>
</template>

<script>
import DateRangePickerCustom from "../customPickers/DateRangePickerCustom.vue";

export default {
  name: "DateRangeInput",
  components: { DateRangePickerCustom },
  props: {
    error: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      internalValue: this.value,
    };
  },
  computed: {
    formattedValue() {
      if (!Array.isArray(this.internalValue)) return "";

      const [from, to] = this.internalValue;
      return from && to
        ? `${new Date(from).toLocaleDateString("ru-RU")} - ${new Date(
            to
          ).toLocaleDateString("ru-RU")}`
        : "";
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
  methods: {},
};
</script>
