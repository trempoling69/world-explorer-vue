<template>
  <div class="chart-container">
    <h2>Exchange rate history for {{ currency }} against EUR</h2>
    <div ref="chart"></div>
  </div>
</template>
<script>
import { createChart } from "lightweight-charts";
import axios from "axios";

let chart = null;
let lineSeries = null;

export default {
  name: "ExchangeRateHistory",
  props: {
    currency: {
      type: String,
      default: "EUR",
    },
  },
  computed: {
    dataUrl: function () {
      return `https://api.exchangerate.host/timeseries?start_date=2022-12-01&end_date=2023-01-10&base=EUR&symbols=${this.currency}`;
    },
  },
  methods: {
    createChart: function () {
      chart = createChart(this.$refs.chart, {
        width: 800,
        height: 600,
      });
    },
    refreshChart: function () {
      axios
        .get(this.dataUrl)
        .then((result) => {
          const data = Object.keys(result.data.rates).map((key) => {
            return {
              time: key,
              value: result.data.rates[key][this.currency],
            };
          });

          if (lineSeries) chart.removeSeries(lineSeries);

          lineSeries = chart.addLineSeries();
          lineSeries.setData(data);
        })
        .catch((error) => console.error(error));
    },
    removeChart: function() {
        if (chart) {
            chart.remove();
            chart = null;
        }
        if (lineSeries)
            lineSeries = null;

    }
  },
  mounted() {
    this.createChart();
    this.refreshChart();
  },
  watch: {
    currency: function () {
      this.refreshChart();
    },
  },
};
</script>