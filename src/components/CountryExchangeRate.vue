<template>
  <div>
    Country currency :
    <currency-selector
      :currencies="countryCurrencies"
      :selected-currency="localCurrency"
      @currency-change="updateLocalCurrency"
    ></currency-selector>
    <exchange-rate-wrapper
      :currencies="currencies"
      :currency="localCurrency"
    ></exchange-rate-wrapper>
  </div>
</template>
<script>
import ExchangeRateWrapper from "./ExchangeRateWrapper.vue";
import CurrencySelector from "./CurrencySelector.vue";
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
    }
  },
  mounted() {
    this.localCurrency = this.currency;
  },
};
</script>