<template>
  <div class="date-picker-wrapper">
    <date-picker
      v-model="tempRange"
      type="daterange"
      :popup-style="{ paddingBottom: '44px' }"
      :editable="false"
      :clearable="false"
      :input-attr="{ readonly: true }"
      :range="true"
      :open="open"
      @open="open = true"
      format="DD.MM.YYYY"
      range-separator=" - "
      placeholder="Выберите период"
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
  name: "DateRangePickerCustom",
  components: { DatePicker },
  props: {
    value: {
      type: Array,
      default: () => [null, null],
    },
    error: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      tempRange: this.value,
      open: false,
    };
  },
  methods: {
    confirm() {
      this.$emit("input", this.tempRange);
      this.open = false;
    },
    cancel() {
      this.tempRange = null;
      this.$emit("input", null);
      this.open = false;
    },
  },
  watch: {
    value(newVal) {
      this.tempRange = newVal;
    },
    internalValue(newVal) {
      this.$emit("input", newVal);
    },
  },
};
</script>
