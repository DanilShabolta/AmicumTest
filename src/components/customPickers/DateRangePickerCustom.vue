<template>
  <div class="date-picker-wrapper">
    <date-picker
      ref="datepicker"
      v-model="tempRange"
      type="daterange"
      :open="open"
      :clearable="false"
      :editable="false"
      :popup-style="{ marginTop: '6px' }"
      popup-class="custom-popup"
      format="dd.MM.yyyy"
      :range="true"
      range-separator=" - "
      :unlink-panels="true"
      @open="open = true"
      @change="handleRangeChange"
    >
      <template #input>
        <div class="custom-input-wrapper" @click.stop>
          <input
            ref="customInput"
            type="text"
            class="custom-input"
            placeholder="ДД.ММ.ГГГГ - ДД.ММ.ГГГГ"
            :value="displayValue"
            @input="handleMaskedInput"
            @keydown.enter="confirm"
            :class="{
              'input-error': error || hasInputError,
              'input-filled': rawInput,
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
    value: { type: Array, default: () => [null, null] },
    error: Boolean,
    clearTrigger: Boolean,
  },
  data() {
    return {
      tempRange: [...this.value],
      rawInput:
        this.value[0] && this.value[1]
          ? `${format(this.value[0], "dd.MM.yyyy")} - ${format(
              this.value[1],
              "dd.MM.yyyy"
            )}`
          : "",
      open: false,
      hasInputError: false,
    };
  },
  computed: {
    displayValue() {
      return this.rawInput || "__.__.____ - __.__.____";
    },
  },
  methods: {
    toggle() {
      this.open = !this.open;
    },
    confirm() {
      const [start, end] = this.tempRange;
      if (!start || !end || isNaN(start) || isNaN(end)) {
        this.hasInputError = true;
        return;
      }

      this.hasInputError = false;
      this.rawInput = `${format(start, "dd.MM.yyyy")} - ${format(
        end,
        "dd.MM.yyyy"
      )}`;
      this.open = false;
      this.$emit("input", [start, end]);
      this.$emit("confirm", [start, end]);
    },
    cancel() {
      this.hasInputError = false;
      this.open = false;
      this.tempRange = [...this.value];
      this.rawInput =
        this.value[0] && this.value[1]
          ? `${format(this.value[0], "dd.MM.yyyy")} - ${format(
              this.value[1],
              "dd.MM.yyyy"
            )}`
          : "";
    },
    handleRangeChange(range) {
      this.tempRange = range;
    },
    handleMaskedInput(e) {
      const digits = e.target.value.replace(/\D/g, "").slice(0, 16);
      const mask = "__.__.____ - __.__.____".split("");

      [...digits].forEach((char, i) => {
        const map = [0, 1, 3, 4, 6, 7, 8, 9, 13, 14, 16, 17, 19, 20, 21, 22];
        if (map[i] !== undefined) mask[map[i]] = char;
      });

      const val = mask.join("");
      this.rawInput = val;
      this.$emit("raw-input", val);
      this.hasInputError = false;

      const parts = val.split(" - ");
      if (parts.length === 2) {
        const start = parse(parts[0], "dd.MM.yyyy", new Date());
        const end = parse(parts[1], "dd.MM.yyyy", new Date());
        if (!isNaN(start) && !isNaN(end)) {
          this.tempRange = [start, end];
        }
      }
    },
  },
  watch: {
    value(newVal) {
      this.tempRange = [...newVal];
      this.rawInput =
        newVal[0] && newVal[1]
          ? `${format(newVal[0], "dd.MM.yyyy")} - ${format(
              newVal[1],
              "dd.MM.yyyy"
            )}`
          : "";
    },
    clearTrigger() {
      this.rawInput = "";
    },
  },
};
</script>
