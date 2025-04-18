<template>
  <div class="date-picker-wrapper">
    <date-picker
      ref="datepicker"
      v-model="tempRange"
      range
      :open="open"
      :clearable="false"
      :editable="range"
      :singleDatePicker="true"
      :split-panels="false"
      :popup-style="{ marginTop: '6px' }"
      popup-class="custom-popup"
      @open="open = true"
      @change="handleDateChange"
    >
      <template #input>
        <div class="custom-input-wrapper" @click.stop>
          <input
            type="text"
            class="custom-input"
            placeholder="ДД.ММ.ГГГГ - ДД.ММ.ГГГГ"
            :value="displayValue"
            @input="handleMaskedInput"
            @keydown.enter="handleEnter"
            :class="{
              'input-error': error || hasInputError,
              'input-filled': rawInput.start || rawInput.end,
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
  name: "DateRangePickerCustom",
  components: { DatePicker },
  props: {
    value: {
      type: Object,
      default: () => ({ start: null, end: null }),
    },
    error: Boolean,
    clearTrigger: Boolean,
    singleDatePicker: String,
  },
  data() {
    return {
      tempRange: [this.value.start, this.value.end],
      rawInput: {
        start: this.value.start ? format(this.value.start, "dd.MM.yyyy") : "",
        end: this.value.end ? format(this.value.end, "dd.MM.yyyy") : "",
      },
      open: false,
      hasInputError: false,
    };
  },
  computed: {
    displayValue() {
      const { start, end } = this.rawInput;
      return start || end
        ? `${start || "__.__.____"} - ${end || "__.__.____"}`
        : "__.__.____ - __.__.____";
    },
  },
  methods: {
    toggle() {
      this.open = !this.open;
    },
    confirm() {
      const parsedStart = parse(this.rawInput.start, "dd.MM.yyyy", new Date());
      const parsedEnd = parse(this.rawInput.end, "dd.MM.yyyy", new Date());

      if (isNaN(parsedStart) || isNaN(parsedEnd)) {
        this.hasInputError = true;
        return;
      }

      this.tempRange = [parsedStart, parsedEnd];
      this.hasInputError = false;
      this.open = false;

      this.rawInput = {
        start: format(parsedStart, "dd.MM.yyyy"),
        end: format(parsedEnd, "dd.MM.yyyy"),
      };

      this.$emit("input", { start: parsedStart, end: parsedEnd });
      this.$emit("confirm", { start: parsedStart, end: parsedEnd });
    },
    cancel() {
      this.hasInputError = false;
      this.open = false;
      this.tempRange = [this.value.start, this.value.end];
      this.rawInput = {
        start: this.value.start ? format(this.value.start, "dd.MM.yyyy") : "",
        end: this.value.end ? format(this.value.end, "dd.MM.yyyy") : "",
      };
    },
    handleEnter() {
      if (
        this.rawInput.start.length === 10 &&
        this.rawInput.end.length === 10
      ) {
        this.confirm();
      }
    },
    handleDateChange([start, end]) {
      this.tempRange = [start, end];
      this.rawInput = {
        start: start ? format(start, "dd.MM.yyyy") : "",
        end: end ? format(end, "dd.MM.yyyy") : "",
      };
      this.$emit("raw-input", { ...this.rawInput });
    },
    handleMaskedInput(e) {
      const raw = e.target.value.replace(/\D/g, "").slice(0, 16);
      const s = raw.slice(0, 8);
      const e_ = raw.slice(8, 16);

      const applyMask = (digits) => {
        const mask = "__.__.____".split("");
        [...digits].forEach((char, i) => {
          const map = [0, 1, 3, 4, 6, 7, 8, 9];
          if (map[i] !== undefined) mask[map[i]] = char;
        });
        return mask.join("");
      };

      this.rawInput.start = applyMask(s);
      this.rawInput.end = applyMask(e_);
      this.hasInputError = false;
      this.$emit("raw-input", { ...this.rawInput });
    },
    clear() {
      this.tempRange = [null, null];
      this.rawInput = { start: "", end: "" };
    },
  },
  watch: {
    value(newVal) {
      this.tempRange = [newVal.start, newVal.end];
      this.rawInput = {
        start: newVal.start ? format(newVal.start, "dd.MM.yyyy") : "",
        end: newVal.end ? format(newVal.end, "dd.MM.yyyy") : "",
      };
    },
    clearTrigger() {
      this.clear();
      this.hasInputError = false;
    },
  },
};
</script>
