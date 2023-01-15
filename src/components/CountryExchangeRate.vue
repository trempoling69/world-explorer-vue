<template>
  <div class="main-container">
    Country currency :
    <currency-selector
      :currencies="countryCurrencies"
      :selected-currency="localCurrency"
      @currencyChange="updateLocalCurrency"
    ></currency-selector>
    <exchange-rate-wrapper
      :currencies="currencies"
      :currency="localCurrency"
      :base-currency="baseCurrency"
      @baseCurrencyChange="forwardBaseCurrencyChange"
    ></exchange-rate-wrapper>
  </div>
</template>
<script>
import ExchangeRateWrapper from "./ExchangeRateWrapper.vue";
import CurrencySelector from "./CurrencySelector.vue";

export default {
  name: "CountryExchangeRate",
  components: { ExchangeRateWrapper, CurrencySelector },
  emits: ["baseCurrentyChange"],
  data() {
    return {
      localCurrency: "",
    };
  },
  props: {
    country: {
      type: Object,
      default: null,
    },
    currencies: {
      type: Array,
      default: () => [],
    },
    currency: {
      type: String,
      default: "",
    },
    baseCurrency: {
      type: String,
      default: ""
    },
  },
  computed: {
    countryCurrencies: function () {
      if (this.country) return Object.keys(this.country.currencies);

      return [];
    },
  },
  methods: {
    updateLocalCurrency(newCurrency) {
        this.localCurrency = newCurrency;
    },
    forwardBaseCurrencyChange: function(newCurrency) {
        this.$emit("baseCurrencyChange", newCurrency);
    }
  },
  mounted() {
    this.localCurrency = this.currency;
  },
};
</script>
<style scoped>
  .main-container {
    background-color: #ffffff;
  }
</style>