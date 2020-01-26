<template lang="pug">
    div.the-gas-station-calc__body.the-gas-station-calc-petrol-calc.the-gas-station-calc-body
        div.the-gas-station-calc-body__part-left
            div.the-gas-station-description.the-gas-station-calc-ml-0.the-gas-station-calc-w-100
                div.the-gas-station-description__title(:class="active_classes.color_class")
                    | {{active_classes.title}}
                div.the-gas-station-description__text
                    | Выберите количество литров для заправки. Исользуйте ползунок, после нажмите
                    span(:class="active_classes.color_class")  продолжить
                    | , чтобы перейти к оплате.

            range-slider.the-gas-station-calc-slider(
                :class="this.active_classes.slider"
                :min="min_liters"
                :max="max_liters"
                step="1"
                v-model="value"
                @change="changeSlider")
            span.the-gas-station-calc-petrol-calc__price(:class="active_classes.color_class") К оплате: $ {{ amount }}</span>
        div.the-gas-station-calc-body__part-right
            TheGasStationProgressRing(:count_litters="value" :active_classes="active_classes")
</template>

<script>
    import RangeSlider from 'vue-range-slider'
    import TheGasStationProgressRing from "./TheGasStationProgressRing";
    import 'vue-range-slider/dist/vue-range-slider.css'

    export default {
        name: "TheGasStationCalc",
        components: {
            RangeSlider,
            TheGasStationProgressRing
        },
        props: {
            active_classes: Object,
            max_liters: {
                default: 40,
                type: Number
            },
            min_liters: {
                default: 20,
                type: Number
            },

        },
        data() {
            return {
                value: 0,
                slider_style: null
            }
        },
        computed: {
            amount() {
                let amount = this.active_classes.price_per_liter * (this.value - this.min_liters);

                return Math.round(amount * 100) / 100;
            }
        },
        methods: {
            changeSlider() {
                this.setAmountEvent();
            },
            setAmountEvent() {
                this.$emit('setAmount', this.amount);
            },
        },
        mounted() {
            this.value = this.min_liters;
        }
    }
</script>

<style lang="scss">
    .the-gas-station-calc-w-100 {
        width: 100% !important;
    }

    .the-gas-station-calc-ml-0 {
        margin-left: 0 !important;
    }

    .the-gas-station-calc-slider {
        width: 95%;
        padding-left: 0;
        margin: 0.25rem 0;

        .range-slider-fill, .range-slider-knob {
            background: #b7ed51;
            border: none;
            height: 0.8125rem;
            width: 0.8125rem;
        }

        .range-slider-fill, .range-slider-rail {
            height: 0.375rem;
        }

        .range-slider-rail {
            background: #42505f;
        }

        .range-slider-rail {
            &:before {
                content: '';
                display: block;
                right: -0.375rem;
                top: -0.25rem;
                position: absolute;
                box-sizing: border-box;
                height: 0.8125rem;
                width: 0.8125rem;
                border-radius: 50%;
                background: #42505f;
            }
        }
    }

    .the-gas-station-calc__body {
        display: flex;
        text-align: left;
        box-sizing: border-box;
        margin-top: 1.25rem;

        .the-gas-station-calc-body__part-left {
            width: 80%;
            margin-left: 2.1875rem;

            .the-gas-station-calc-petrol-calc__price {
                display: block;
                text-align: center;
                margin-left: 1.625rem;
                margin-top: 0.1875rem;
                width: 100%;
                font-weight: bold;
                font-size: 1rem;
            }
        }
    }
</style>