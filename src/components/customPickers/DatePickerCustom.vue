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
            class="custom-input"
            placeholder="ДД.ММ.ГГГГ"
            @focus.prevent
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
import IMask from "imask";
import { isValid } from "date-fns";

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
      mask: null,
    };
  },
  mounted() {
    this.initMask();
  },
  methods: {
    initMask() {
      const input = this.$refs.customInput;
      this.mask = IMask(input, {
        mask: Date,
        pattern: "d.`m.`Y",
        lazy: false,
        autofix: true,
        overwrite: true,
        blocks: {
          d: {
            mask: IMask.MaskedRange,
            from: 1,
            to: 31,
            maxLength: 2,
            placeholderChar: "Д",
          },
          m: {
            mask: IMask.MaskedRange,
            from: 1,
            to: 12,
            maxLength: 2,
            placeholderChar: "М",
          },
          Y: {
            mask: IMask.MaskedRange,
            from: 1900,
            to: 2099,
            placeholderChar: "Г",
          },
        },
        format: (date) => {
          return date.toLocaleDateString("ru-RU");
        },
        parse: (str) => {
          const [day, month, year] = str.split(".").map(Number);
          return new Date(year, month - 1, day);
        },
      });

      this.mask.on("accept", () => {
        const parsed = this.mask.typedValue;
        if (parsed && isValid(parsed)) {
          this.tempDate = parsed;
        }
      });
    },
    handleDateChange(date) {
      this.tempDate = date;
    },
    confirm() {
      this.$emit("input", this.tempDate);
      this.$emit("confirm", this.tempDate);
      this.open = false;
      if (this.mask) {
        this.mask.typedValue = this.tempDate;
      }
    },
    cancel() {
      this.open = false;
      this.tempDate = this.value;
      if (this.mask) {
        this.mask.typedValue = this.tempDate;
      }
    },
    toggle() {
      this.open = !this.open;
    },
  },
  watch: {
    value(newVal) {
      this.tempDate = newVal;
      if (this.mask && isValid(newVal)) {
        this.mask.typedValue = newVal;
      }
    },
  },
};
</script>
