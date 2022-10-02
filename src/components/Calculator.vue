<template>
  <div class="d-flex align-items-center justify-content-center h-100">
    <div class="container">
      <h1 class="calculator__title col-12 col-xl-6 col-md-10">Рассчитайте стоимость автомобиля в лизинг</h1>
      <form ref="form" @submit.prevent="submitData" action="">
        <div class="row mb-1">
          <div class="col-12 col-xl-4">
            <div class="mb-2 mb-sm-4">Стоимость автомобиля</div>
            <range-input
              :min-value="1000000"
              :max-value="6000000"
              :step="10000"
              v-model="costCar"
              :name="'costCar'"
            >
              ₽
            </range-input>
          </div>
          <div class="col-12 col-xl-4">
            <div class="mb-2 mb-sm-4">Первоначальный взнос</div>
            <range-input
                :min-value="10"
                :max-value="60"
                :step="1"
                v-model="firstContribution"
                :first-contribution="true"
                :first-contribution-value="firstContributionValue"
                :name="'firstContribution'"
            />
          </div>
          <div class="col-12 col-xl-4">
            <div class="mb-2 mb-sm-4">Срок лизинга</div>
            <range-input
                :min-value="1"
                :max-value="60"
                :step="1"
                v-model="period"
                :name="'period'"
            >
              мес.
            </range-input>
          </div>
        </div>
        <div class="row align-items-end">
          <div class="calculator__sum_credit col-12 col-xl-4 col-sm-6">
            <div class="calculator__result-text mb-2">Сумма договора лизинга</div>
            <div class="calculator__result-value">
              <span>{{getFullSum.toLocaleString('ru')}}</span>
              <span class="mx-3">₽</span>
            </div>
          </div>
          <div class="calculator__month_value col-12 col-xl-4 col-sm-6">
            <div class="calculator__result-text mb-2">Ежемесячный платеж от</div>
            <div class="calculator__result-value">
              <span>{{monthlyPayment.toLocaleString('ru')}}</span>
              <span class="mx-3">₽</span>
            </div>
          </div>
          <div class="col-12 col-lg-5 col-xl-4 col-sm-6">
            <button
                :disabled="disabledButton"
                type="submit"
                class="calculator__send_btn w-100 d-flex justify-content-center align-items-center"
                :class="{'loading': isLoading}"
            >
              Оставить заявку
              <div class="disabled-element"></div>
            </button>
          </div>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import RangeInput from "@/components/UI/RangeInput";

export default {
  name: "Calculator",
  data () {
    return {
      costCar: null,
      firstContribution: null,
      period: null,
      disabledButton: false,
      isLoading: false,
    }
  },
  components: {
    RangeInput,
  },
  computed: {
    getFullSum () {
      return this.firstContributionValue + this.period * this.monthlyPayment
    },
    firstContributionValue () {
      return this.costCar / 100 * this.firstContribution
    },
    monthlyPayment () {
      return ((this.costCar - this.firstContributionValue) * ((0.035 * Math.pow((1 + 0.035), this.period)) / (Math.pow((1 + 0.035), this.period) - 1))).toFixed()
    },
  },
  methods: {
    submitData () {
      const form = new FormData(this.$refs.form);
      let costCar = +form.get('costCar');
      let firstContribution = +form.get('firstContribution');
      let period = +form.get('period');
      let getFullSum = +this.getFullSum;
      let monthlyPayment = +this.monthlyPayment;
      const requestData = {
        costCar,
        firstContribution,
        period,
        getFullSum,
        monthlyPayment,
      };

      this.disabledButton = true;
      this.isLoading = true;
      fetch('https://eoj3r7f3r4ef6v4.m.pipedream.net', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json;charset=utf-8'
        },
        body: JSON.stringify(requestData)
      })
          .finally(() => {
            this.disabledButton = false;
            this.isLoading = false;
          })
    },
  }
}
</script>

<style scoped>

</style>
