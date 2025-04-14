<template>
  <div class="date-picker-wrapper">
    <date-picker
      ref="datepicker"
      v-model="tempDateTime"
      type="datetime"
      format="DD.MM.YYYY HH:mm"
      :open="open"
      :clearable="false"
      :editable="false"
      :popup-style="{ marginTop: '6px' }"
      :input-attr="{ readonly: true }"
      :class="[value ? 'filled' : 'empty', error ? 'error' : '']"
      @open="open = true"
      @change="handleDateTimeChange"
      placeholder="Выберите дату и время"
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
  name: "DateTimePickerCustom",
  components: { DatePicker },
  props: {
    value: { type: Date, default: () => null },
    error: { type: Boolean, default: false },
  },
  data() {
    return {
      tempDateTime: this.value,
      open: false,
    };
  },
  methods: {
    confirm() {
      this.$emit("input", this.tempDateTime);
      this.open = false;
    },
    cancel() {
      this.tempDateTime = null;
      this.open = false;
    },
    handleDateTimeChange(value) {
      this.tempDateTime = value;
    },
  },
  watch: {
    value(newVal) {
      this.tempDateTime = newVal;
    },
  },
};
</script>
