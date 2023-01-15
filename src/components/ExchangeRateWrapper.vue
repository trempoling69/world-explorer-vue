<template>
  <div class="chart-container">
    Base currency :
    <currency-selector
      :currencies="currencies"
      :selectedCurrency="baseCurrency"
      @currencyChange="forwardCurrencyChange"
    ></currency-selector>
    <exchange-rate-history
      :currency="currency"
      :base-currency="baseCurrency"
    ></exchange-rate-history>
  </div>
</template>
<script>
import CurrencySelector from './CurrencySelector.vue';
import ExchangeRateHistory from './ExchangeRateHistory.vue';

export default {
  name: "ExchangeRateWrapper",
  components: {
    CurrencySelector,
    ExchangeRateHistory
  },
  emits: ["baseCurrencyChange"],
  props: {
    currencies: {
        type: Array,
        default: () => []
    },
    currency: {
        type: String,
        default: "EUR"
    },
    baseCurrency: {
        type: String,
        default: "EUR"
    }
  },
  methods: {
    forwardCurrencyChange: function(newCurrency) {
        this.$emit("baseCurrencyChange", newCurrency);
    }
  }
};
</script>
<style scoped>
  .chart-container {
    background-color: #ffffff;
  }
</style>