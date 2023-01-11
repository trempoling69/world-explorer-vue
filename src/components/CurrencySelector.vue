<template>
    <div>
        <select v-model="selected">
            <option v-for="currency in currencies" :value="currency" :key="currency">
                {{  currency }}
            </option>
        </select>
    </div>
</template>
<script>
    export default {
        name: "CurrencySelector",
        props: {
            currencies: {
                type: Array,
                default: () => []
            },
            selectedCurrency: {
                type: String,
                default: "EUR"
            }
        },
        emits: ["currencyChange"],
        data() {
            return {
                selected: ""
            }
        },
        mounted() {
            this.selected = this.selectedCurrency;
        },
        watch: {
            selectedCurrency: function(newCurrency) {
                this.selected = newCurrency;
            },
            selected: function(newCurrency) {
                this.$emit("currencyChange", newCurrency);
            }
        }
    }
</script>