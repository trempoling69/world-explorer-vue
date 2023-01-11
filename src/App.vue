<template>
  <div>
    <h2>{{ title }}</h2>
    <input type="text" v-model="filter" />
    <currency-selector
      :currencies="allCurrencies"
      :selectedCurrency="baseCurrency"
      @currencyChange="updateBaseCurrency"
    ></currency-selector>
    <table class="table">
      <theader>
        <tr>
          <th>Name</th>
          <th>Languages</th>
          <th>Currencies</th>
        </tr>
      </theader>
      <tbody>
        <tr v-for="country in filteredCountries" v-bind:key="country.fifa">
          <td>
            <button class="button is-ghost" @click="flag = country.flags.png">
              {{ country.name.common }}
            </button>
          </td>
          <td>
            <ul>
              <li v-for="language in country.languages" :key="language">
                {{ language }}
              </li>
            </ul>
          </td>
          <td>
            <ul>
              <li v-for="(cur, key) in country.currencies" :key="key">
                <button class="button is-ghost" @click="currency = key">
                  {{ key }}
                </button>
              </li>
            </ul>
          </td>
        </tr>
      </tbody>
    </table>
    <div class="modal" :class="{ 'is-active': isFlag }">
      <div class="modal-background"></div>
      <div class="modal-content">
        <p class="image is-4by3">
          <img :src="flag" alt="" />
        </p>
      </div>
      <button
        class="modal-close is-large"
        aria-label="close"
        @click="flag = ''"
      ></button>
    </div>
    <div class="modal" :class="{ 'is-active': isCurrency }">
      <div class="modal-background"></div>
      <div class="modal-content">
        <div class="chart-container">
          Base currency :
          <currency-selector
            :currencies="allCurrencies"
            :selectedCurrency="baseCurrency"
            @currencyChange="updateBaseCurrency"
          ></currency-selector>
          <exchange-rate-history
            :currency="currency"
            :base-currency="baseCurrency"
          ></exchange-rate-history>
        </div>
      </div>
      <button
        class="modal-close is-large"
        aria-label="close"
        @click="currency = ''"
      ></button>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import ExchangeRateHistory from "./components/ExchangeRateHistory.vue";
import CurrencySelector from "./components/CurrencySelector.vue";

export default {
  name: "App",
  components: { ExchangeRateHistory, CurrencySelector },
  data() {
    return {
      title: "World Explorer",
      countries: [],
      filter: "",
      flag: "",
      currency: "",
      baseCurrency: "EUR",
    };
  },
  methods: {
    fetchCountries: function () {
      axios
        .get("https://restcountries.com/v3.1/all")
        .then((response) => (this.countries = response.data))
        .catch((error) => console.error(error));
    },
    updateBaseCurrency(newCurrency) {
      this.baseCurrency = newCurrency;
    },
  },
  computed: {
    sortedCountries: function () {
      return [...this.countries].sort((c1, c2) =>
        c1.name.common.localeCompare(c2.name.common)
      );
    },
    filteredCountries: function () {
      return this.sortedCountries.filter((c) =>
        c.name.common.toLowerCase().includes(this.filter.toLowerCase())
      );
    },
    isFlag: function () {
      return this.flag !== "";
    },
    isCurrency: function () {
      return this.currency !== "";
    },
    allCurrencies: function () {
      if (this.countries.length > 0) {
        const currencies = [];

        this.countries.forEach((country) => {
          if (country.currencies)
            currencies.push(...Object.keys(country.currencies));
        });
        return [...new Set(currencies)].sort();
      }

      return [];
    },
  },
  mounted() {
    this.fetchCountries();
  },
};
</script>

<style>
@import "~bulma/css/bulma.css";
</style>

<style scoped>
  .chart-container {
    background-color: #ffffff;
  }
</style>
