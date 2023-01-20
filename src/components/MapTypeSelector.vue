<template>
<select v-model="selected"> 
    <option v-for="style in AllType" :value="style[1]" :key="style[1]">
                {{  style[0] }}
            </option>
</select>
</template>

<script>
export default{
    name: "MapTypeSelector",
        props: {
            selectedType : {
                type : String,
                default : 'm'
            }
            // currencies: {
            //     type: Array,
            //     default: () => []
            // },
            // selectedCurrency: {
            //     type: String,
            //     default: "EUR"
            // }
        },
        emits: ["typeChange"],
        data() {
            return {
                AllType : new Map([["Normal","m"], ["Satellite","k"], ["Hybrid","h"], ["Terrain","p"]]),
                selected: ""
            }
        },
        mounted() {
            this.selected = this.selectedType;
        },
        watch: {
            selectedType: function(newType) {
                this.selected = newType;
            },
            selected: function(newType) {
                this.$emit("typeChange", newType);
            }
        }
}
</script>