<template>
  <div class="date-picker-wrapper">
    <date-picker
      ref="datepicker"
      v-model="tempDate"
      :open="open"
      :clearable="false"
      :editable="true"
      :popup-style="{ marginTop: '6px' }"
      popup-class="custom-popup"
      @open="open = true"
      @change="handleDateChange"
    >
      <template #input>
        <div class="custom-input-wrapper" @click.stop>
          <input
            ref="customInput"
            type="text"
            class="custom-input"
            placeholder="ДД.ММ.ГГГГ"
            :value="displayValue"
            @input="handleMaskedInput"
            @keydown.enter="confirm"
            :class="{
              'input-error': error || hasInputError,
              'input-filled': rawInput || tempDate,
            }"
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
    error: Boolean,
    clearTrigger: Boolean,
  },
  data() {
    return {
      tempDate: this.value,
      rawInput: this.value ? format(this.value, "dd.MM.yyyy") : "",
      open: false,
      hasInputError: false,
    };
  },
  computed: {
    displayValue() {
      if (this.rawInput) return this.rawInput;
      if (this.tempDate) return format(this.tempDate, "dd.MM.yyyy");
      return "__.__.____";
    },
  },
  methods: {
    toggle() {
      this.open = !this.open;
    },
    confirm() {
      let parsed;

      if (this.rawInput) {
        parsed = parse(this.rawInput, "dd.MM.yyyy", new Date());
      } else if (this.tempDate) {
        parsed = this.tempDate;
        this.rawInput = format(parsed, "dd.MM.yyyy");
      }

      if (!parsed || isNaN(parsed)) {
        this.hasInputError = true;
        return;
      }

      this.tempDate = parsed;
      this.hasInputError = false;
      this.open = false;

      this.$emit("input", this.tempDate);
      this.$emit("confirm", this.tempDate);
    },
    cancel() {
      this.hasInputError = false;
      this.open = false;
      this.tempDate = this.value;
      this.rawInput = this.value ? format(this.value, "dd.MM.yyyy") : "";
    },
    handleDateChange(date) {
      this.tempDate = date;
    },
    handleMaskedInput(e) {
      const digits = e.target.value.replace(/\D/g, "").slice(0, 8);
      const mask = "__.__.____".split("");

      [...digits].forEach((char, i) => {
        const map = [0, 1, 3, 4, 6, 7, 8, 9];
        if (map[i] !== undefined) mask[map[i]] = char;
      });

      const val = mask.join("");
      this.rawInput = val;
      this.$emit("raw-input", val);
      this.hasInputError = false;

      const parsed = parse(val, "dd.MM.yyyy", new Date());
      if (!isNaN(parsed)) {
        this.tempDate = parsed;
      }
    },
  },
  watch: {
    value(newVal) {
      this.tempDate = newVal;
      this.rawInput = newVal ? format(newVal, "dd.MM.yyyy") : "";
    },
    clearTrigger() {
      this.rawInput = "";
    },
  },
};
</script>
