<template>
  <div class="date-picker-wrapper">
    <date-picker
      ref="datepicker"
      v-model="tempDate"
      :open="open"
      :clearable="false"
      :editable="true"
      :popup-style="{ marginTop: '6px' }"
      :popup-class="'custom-popup'"
      @open="open = true"
      @change="handleDateChange"
    >
      <template #footer>
        <div class="custom-footer">
          <button class="confirm-btn" @click="confirm">Подтвердить</button>
          <button class="cancel-btn" @click="cancel">Отменить</button>
        </div>
      </template>
    </date-picker>
  </div>
</template>

<script>
import DatePicker from "vue2-datepicker";
import "vue2-datepicker/index.css";

export default {
  name: "DatePickerCustom",
  components: { DatePicker },
  props: {
    value: { type: Date, default: null },
  },
  data() {
    return {
      tempDate: this.value,
      open: false,
    };
  },
  methods: {
    confirm() {
      this.$emit("input", this.tempDate);
      this.$emit("confirm", this.tempDate); // <-- добавь эту строку
      this.open = false;
    },
    cancel() {
      this.open = false;
      this.tempDate = this.value; // ← вернуть к исходной дате
    },

    handleDateChange(value) {
      this.tempDate = value;
    },
  },
  watch: {
    value(newVal) {
      this.tempDate = newVal;
    },
  },
};
</script>
