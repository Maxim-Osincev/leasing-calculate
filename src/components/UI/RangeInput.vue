<template>
  <label class="inputs__wrapper w-100" :for="id">
    <input
        class="input-number"
        :id="id"
        v-model="value"
        @blur="verifyValue"
        type="number"
        :min="minValue"
        :max="maxValue"
    >
    <input
        class="input-range"
        v-model="value"
        type="range"
        :min="minValue"
        :max="maxValue"
        :step="step"
    >
  </label>
</template>

<script>
export default {
  name: "RangeInput",
  data () {
    return {
      value: this.minValue,
      id: null,
    }
  },
  props: {
    modelValue: {},
    firstContributionValue: {
      type: Number,
    },
    minValue: {
      required: true,
      type: Number,
    },
    maxValue: {
      required: true,
      type: Number,
    },
    step: {
      required: false,
      type: Number,
      default: 1,
    },
  },
  methods: {
    verifyValue () {
      if (this.value > this.maxValue)  this.value = this.maxValue;
      if (this.value < this.minValue)  this.value = this.minValue;
    }
  },
  watch: {
    value (newValue) {
      this.$emit('update:modelValue', +newValue)
    }
  },
  mounted() {
    this.$emit('update:modelValue', +this.value);
    this.id = this._uid
  }
}
</script>

<style scoped>

</style>
