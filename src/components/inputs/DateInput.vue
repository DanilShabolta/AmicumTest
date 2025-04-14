<template>
  <div class="date-input">
    <div class="input-wrapper">
      <input
        ref="input"
        v-model="manualInput"
        class="manual-input"
        placeholder="дд.мм.гггг"
        @input="handleInput"
      />
      <span v-if="manualInput" class="clear-icon" @click="clearInput">×</span>
      <span class="calendar-icon" @click="toggleCalendar"> 📅 </span>
      <date-picker-custom
        v-if="showCalendar"
        v-model="tempValue"
        @confirm="handleConfirm"
        @cancel="showCalendar = false"
      />
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
  },
  data() {
    return {
      manualInput: "",
      tempValue: null,
      showCalendar: false,
    };
  },
  watch: {
    value(val) {
      this.manualInput = val ? new Date(val).toLocaleDateString("ru-RU") : "";
    },
  },
  mounted() {
    if (this.value) {
      this.manualInput = new Date(this.value).toLocaleDateString("ru-RU");
    }
  },
  methods: {
    handleInput(e) {
      let val = e.target.value.replace(/\D/g, "").slice(0, 8);
      if (val.length >= 5) {
        val = val.replace(/(\d{2})(\d{2})(\d{0,4})/, "$1.$2.$3");
      } else if (val.length >= 3) {
        val = val.replace(/(\d{2})(\d{0,2})/, "$1.$2");
      }
      this.manualInput = val;
    },
    toggleCalendar() {
      this.showCalendar = !this.showCalendar;
    },
    handleConfirm(date) {
      this.$emit("input", date);
      this.manualInput = new Date(date).toLocaleDateString("ru-RU");
      this.showCalendar = false;
    },
    clearInput() {
      this.manualInput = "";
      this.$emit("input", null);
    },
  },
};
</script>

<style scoped>
.input-wrapper {
  position: relative;
  display: flex;
  align-items: center;
}

.manual-input {
  width: 100%;
  padding-right: 60px;
  font-size: 14px;
}

.clear-icon {
  position: absolute;
  right: 30px;
  cursor: pointer;
  font-size: 18px;
  color: #999;
}

.calendar-icon {
  position: absolute;
  right: 8px;
  cursor: pointer;
  font-size: 16px;
}
</style>
