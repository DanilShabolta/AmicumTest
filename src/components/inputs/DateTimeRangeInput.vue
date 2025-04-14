<template>
  <div class="date-time-range-wrapper">
    <div class="range-block">
      <div class="range-part">
        <label>с</label>
        <date-picker
          ref="fromPicker"
          v-model="range[0]"
          type="datetime"
          format="DD.MM.YYYY HH:mm"
          :editable="false"
          :clearable="false"
          @change="emitChange"
          :class="[range[0] ? 'filled' : 'empty', fromError ? 'error' : '']"
        />
      </div>
      <div class="range-part">
        <label>по</label>
        <date-picker
          ref="toPicker"
          v-model="range[1]"
          type="datetime"
          format="DD.MM.YYYY HH:mm"
          :editable="false"
          :clearable="false"
          @change="emitChange"
          :class="[range[1] ? 'filled' : 'empty', toError ? 'error' : '']"
        />
      </div>
    </div>
  </div>
</template>

<script>
import DatePicker from "vue2-datepicker";

export default {
  name: "DateTimeRangeInput",
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
  computed: {
    fromError() {
      return this.error && !this.range[0];
    },
    toError() {
      return this.error && !this.range[1];
    },
  },

  data() {
    return {
      range: {
        from: this.value.from || null,
        to: this.value.to || null,
      },
    };
  },
  watch: {
    value(val) {
      this.range = { ...val };
    },
  },
  methods: {
    emitChange() {
      this.$emit("input", this.range);
    },
  },
};
</script>

<style scoped>
.date-time-range-wrapper {
  background-color: #e7eaf3;
  padding: 10px 12px;
  border-radius: 6px;
  display: inline-block;
}

.range-block {
  display: flex;
  flex-direction: column;
  gap: 10px;
}

.range-part {
  display: flex;
  flex-direction: column;
}

.range-part label {
  font-size: 10px;
  color: #8891a8;
  margin-bottom: 2px;
}

.dark-theme .date-time-range-wrapper {
  background-color: #2f3a5a;
}
</style>
