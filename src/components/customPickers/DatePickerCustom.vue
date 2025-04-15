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
      <template #input>
        <div class="custom-input-wrapper" @click.stop>
          <input
            ref="customInput"
            type="text"
            :value="formattedDate"
            @input="onInput"
            @focus.prevent
            @keydown.enter="confirm"
            placeholder="ДД.ММ.ГГГГ"
            class="custom-input"
          />
        </div>
      </template>
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
import { format, parse } from "date-fns";

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
  computed: {
    formattedDate() {
      return this.tempDate ? format(this.tempDate, "dd.MM.yyyy") : "";
    },
  },
  methods: {
    confirm() {
      this.$emit("input", this.tempDate);
      this.$emit("confirm", this.tempDate);
      this.open = false;
    },
    cancel() {
      this.open = false;
      this.tempDate = this.value;
    },
    handleDateChange(value) {
      this.tempDate = value;
    },
    onInput(e) {
      const inputVal = e.target.value;
      this.$emit("input-change", inputVal);
      const parsed = parse(inputVal, "dd.MM.yyyy", new Date());
      if (!isNaN(parsed)) {
        this.tempDate = parsed;
      }
    },
  },
  watch: {
    value(newVal) {
      this.tempDate = newVal;
    },
  },
};
</script>
