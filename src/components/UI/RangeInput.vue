<template>
  <label
      :class="['inputs__wrapper', 'w-100', {'focused': focus}, {'disabled': disable}, {'isFirstContribution': firstContribution}]"
      :for="id"
  >
    <input
        class="input-number w-100"
        :id="id"
        v-model="value"
        @blur="verifyValue"
        @input="verifyMaxValue"
        type="number"
        :name="name"
        :min="minValue"
        :max="maxValue"
        @focus="this.focus = true"
        :disabled="disable"
    >
    <div class="input-helper">
      <slot></slot>
    </div>
    <div class="contribution-value" v-if="firstContributionValue">
      {{firstContributionValue}}
    </div>
    <input
        class="input-range"
        v-model="value"
        type="range"
        :min="minValue"
        :max="maxValue"
        :step="step"
        :disabled="disable"
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
      focus: false,
      disable: false,
    }
  },
  props: {
    modelValue: {},
    firstContributionValue: {
      type: Number,
    },
    firstContribution: {
      type: Boolean,
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
    name: {
      type: String,
    }
  },
  methods: {
    verifyValue () {
      this.verifyMaxValue ()
      this.verifyMinValue()
      this.focus = false
    },
    verifyMinValue () {
      if (this.value < this.minValue)  this.value = this.minValue;
    },
    verifyMaxValue () {
      if (this.value > this.maxValue)  this.value = this.maxValue;
    }
  },
  watch: {
    value (newValue) {
      this.$emit('update:modelValue', +newValue)
    }
  },
  mounted() {
    this.$emit('update:modelValue', this.value);
    this.id = this._uid
  }
}
</script>

<style scoped>

</style>
