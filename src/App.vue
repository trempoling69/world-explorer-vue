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
      <thead>
        <tr>
          <th>Name</th>
          <th>Languages</th>
          <th>Currencies</th>
          <th>Map</th>
        </tr>
      </thead>
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
          <td>
            <button class="button is-ghost" @click="openMap(country)">
              Map
            </button>
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
    <div class="modal" :class="{ 'is-active': showCurrency }">
      <div class="modal-background"></div>
      <div class="modal-content">
        <exchange-rate-wrapper
          :currencies="allCurrencies"
          :currency="currency"
          :baseCurrency="baseCurrency"
          @baseCurrencyChange="updateBaseCurrency"
        >
        </exchange-rate-wrapper>
      </div>
      <button
        class="modal-close is-large"
        aria-label="close"
        @click="currency = ''"
      ></button>
    </div>
    <div class="modal" :class="{ 'is-active': showMap }">
      <div class="modal-background"></div>
      <div class="modal-content">
        <div>
          <embedded-map :query="mapQuery" :zoom="mapZoom"></embedded-map>
        </div>
      </div>
      <button
        class="modal-close is-large"
        aria-label="close"
        @click="closeMap"
      ></button>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import ExchangeRateWrapper from "./components/ExchangeRateWrapper.vue";
import CurrencySelector from "./components/CurrencySelector.vue";
import EmbeddedMap from "./components/EmbeddedMap.vue";

export default {
  name: "App",
  components: { ExchangeRateWrapper, CurrencySelector, EmbeddedMap },
  data() {
    return {
      title: "World Explorer",
      countries: [],
      filter: "",
      flag: "",
      currency: "",
      baseCurrency: "EUR",
      mapQuery: "",
      mapZoom: 2,
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
    getAreaZoom(area) {
      if (area >= 10000000) return 2;

      if (area >= 1000000) return 3;

      if (area >= 500000) return 4;

      if (area >= 100000) return 5;

      if (area >= 50000) return 6;

      return 9;
    },
    openMap: function (country) {
      this.mapZoom = this.getAreaZoom(country.area);
      this.mapQuery = country.name.common;
    },
    closeMap: function () {
      this.mapQuery = "";
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
    showCurrency: function () {
      return this.currency !== "";
    },
    showMap: function () {
      return this.mapQuery !== "";
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
