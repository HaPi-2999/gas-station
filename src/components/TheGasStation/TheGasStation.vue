<template lang="pug">
    div
        TheGasStationModalsWindows(
            :amount="amount"
            :mainWindow="$refs.price_calc"
            ref="modal_window")
        div#the-gas-station.the-gas-station(ref="price_calc")
            div.the-gas-station-header.the-gas-station__header
                img(src="./assets/logo.png" alt="logo")
                div.the-gas-station-header__bottom-lines
                    div.the-gas-station-header__white-line
                    div.the-gas-station-header__red-line
                    div.the-gas-station-header__white-line
            div.the-gas-station__content.the-gas-station-content
                div.the-gas-station-content__title Выберите тип бензина:
                div.the-gas-station-content__petrol-type.the-gas-station-calc-petrol-calc(
                    @click="setActive('unleaded')"
                    :class="{'the-gas-station-active': active === 'unleaded'}")
                    div.the-gas-station-calc-petrol-calc__logo
                        img(src="./assets/fuel-unleaded.svg" alt="")
                    div.the-gas-station-description
                        div.the-gas-station-description__title.the-gas-station-unleaded Unleaded 87
                        div.the-gas-station-description__text
                            | Бензин низкого качества, отсутствуют какие
                            | либо присатки.
                            | Достаточно хороший вариант для среднего класса машин.
                div.the-gas-station-content__petrol-type.the-gas-station-calc-petrol-calc(
                    @click="setActive('performance_plus')"
                    :class="{'the-gas-station-active': active === 'performance_plus'}")
                    div.the-gas-station-calc-petrol-calc__logo
                        img(src="./assets/fuel-perfomance-plus.svg" alt="")
                    div.the-gas-station-description
                        div.the-gas-station-description__title.the-gas-station-performance-plus Performance Plus
                        div.the-gas-station-description__text
                            | Бензин среднего качества, Добавлена слабая
                            | присадка. Двигатель
                            | становится чуть мощнее, что даёт возможность передвигаться уверенее.
                div.the-gas-station-content__petrol-type.the-gas-station-calc-petrol-calc(
                    @click="setActive('performance_premium')"
                    :class="{'the-gas-station-active': active === 'performance_premium'}")
                    div.the-gas-station-calc-petrol-calc__logo
                        img(src="./assets/fuel-perfomance-premium.svg" alt="")
                    div.the-gas-station-description
                        div.the-gas-station-description__title.the-gas-station-performance-premium Performance PREMIUM
                        div.the-gas-station-description__text
                            | Бензин высокого качества, есть особый состав
                            | присадки, который
                            | увеличивает мощность двигателя в разы. Минимальный износ двигателя.
                TheGasStationCalc(
                    :max_liters="40"
                    :min_liters="20"
                    :active_classes="color_and_background"
                    v-on:setAmount="setAmount")
            div.the-gas-station__button#proceed(:class="color_and_background.background_class"
                @click="proceed($event)") Продолжить
</template>

<script>
    import TheGasStationCalc from "./components/TheGasStationCalc";
    import TheGasStationModalsWindows from "./components/TheGasStationModalsWindows";

    export default {
        name: "TheGasStation",
        components: {
            TheGasStationCalc,
            TheGasStationModalsWindows
        },
        data() {
            return {
                active: 'unleaded',
                show_modals_window: false,
                color_and_background: {},
                amount: 2
            }
        },
        methods: {
            setActive(key) {
                this.active = key;
                this.color_and_background = this.getColorAndBackground();
            },
            getColorAndBackground() {
                switch (this.active) {
                    case 'unleaded':
                        return {
                            title: 'Unleaded 87',
                            color_class: 'the-gas-station-unleaded',
                            stroke_color: 'the-gas-station-stroke-unleaded',
                            background_class: 'the-gas-station-bg-unleaded',
                            price_per_liter: 13,
                            slider: 'the-gas-station-range-slider-unleaded'
                        };
                    case 'performance_plus':
                        return {
                            title: 'Performance Plus',
                            color_class: 'the-gas-station-performance-plus',
                            stroke_color: 'the-gas-station-stroke-performance-plus',
                            background_class: 'the-gas-station-bg-performance-plus',
                            price_per_liter: 16.3,
                            slider: 'the-gas-station-range-slider-performance-plus'
                        };
                    case 'performance_premium':
                        return {
                            title: 'Performance PREMIUM',
                            color_class: 'the-gas-station-performance-premium',
                            stroke_color: 'the-gas-station-stroke-performance-premium',
                            background_class: 'the-gas-station-bg-performance-premium',
                            price_per_liter: 21,
                            slider: 'the-gas-station-range-slider-performance-premium'
                        };
                }

                return {}
            },
            proceed(event) {
                event.stopPropagation();

                let modal = this.$refs.modal_window.getData().modal_window,
                    mainWindow = this.$refs.price_calc;

                mainWindow.classList.add('the-gas-station-blur');
                modal.classList.add('the-gas-station-modal-active');
            },

            setAmount(amount) {
                this.amount = amount;
            }
        },

        mounted() {
            this.color_and_background = this.getColorAndBackground();
        }
    }
</script>

<style lang="scss">
    $bg-header: linear-gradient(180deg, rgba(34, 64, 98, 1) 0%, rgba(61, 101, 147, 1) 100%);
    $white: #fff;
    $red: #eb0f0f;
    $bg-content: #132437;

    $unleaded: #eb0f0f;
    $performance-plus: #52c8e5;
    $performance-premium: #b7ed51;

    @font-face {
        font-family: Gilroy;
        src: url("fonts/Gilroy-Regular.ttf");
    }

    * {
        font-family: Gilroy, serif;
    }


    .the-gas-station-active {
        background: linear-gradient(90deg, rgba(45, 60, 77, 1) 0%, rgba(255, 255, 255, 0) 100%);
        border-radius: 0.625rem;
        padding-left: 0.3125rem !important;
        margin-left: -0.3125rem;
    }

    .the-gas-station-blur {
        filter: blur(3px);
    }

    .the-gas-station__button {
        cursor: pointer;
        margin: 0.375rem auto;
        border-radius: 0.9375rem;
        line-height: 2rem;
        color: #132437;
        width: 15rem;
        height: 2rem;
        font-size: 1rem;
        font-weight: bold;
    }

    .the-gas-station-unleaded {
        color: $unleaded;
    }

    .the-gas-station-bg-unleaded {
        background: $unleaded !important;
    }

    .the-gas-station-stroke-unleaded {
        stroke: $unleaded;
    }

    .the-gas-station-range-slider-unleaded {
        .range-slider-fill, .range-slider-knob {
            background: $unleaded !important;
            box-shadow: 0 0 7px 2px rgba(235, 15, 15, 0.66);
        }
    }

    .the-gas-station-performance-plus {
        color: $performance-plus;
    }

    .the-gas-station-bg-performance-plus {
        background: $performance-plus !important;
    }

    .the-gas-station-range-slider-performance-plus {
        .range-slider-fill, .range-slider-knob {
            background: $performance-plus !important;
            box-shadow: 0 0 7px 2px rgba(82, 200, 229, 0.66);
        }
    }

    .the-gas-station-stroke-performance-plus {
        stroke: $performance-plus;
    }

    .the-gas-station-performance-premium {
        color: $performance-premium;
    }

    .the-gas-station-bg-performance-premium {
        background: $performance-premium !important;
    }

    .the-gas-station-range-slider-performance-premium {
        .range-slider-fill, .range-slider-knob {
            background: $performance-premium !important;
            box-shadow: 0 0 7px 2px rgba(183, 237, 81, 0.66);
        }
    }

    .the-gas-station-stroke-performance-premium {
        stroke: $performance-premium;
    }


    .the-gas-station-calc-petrol-calc {
        padding: 0.3125rem 0;

        .the-gas-station-description {
            margin-left: 0.625rem;
            width: 80%;
            text-align: left;

            .the-gas-station-description__title {
                font-size: 1.125rem;
                font-weight: bold;
                margin-top: 0.3125rem;
            }

            .the-gas-station-description__text {
                font-size: 0.8125rem;
                line-height: 0.8125rem;
            }
        }
    }

    .the-gas-station {
        width: 100%;
        height: auto;
        color: $white;

        .the-gas-station-header {
            position: relative;
            background: $bg-header;
            width: 100%;
            height: 7.0625rem;
            border-top-left-radius: 0.625rem;
            border-top-right-radius: 0.625rem;

            &:before {
                content: url("./assets/uzor.png");
                width: 100%;
            }

            img {
                position: relative;
                margin: -5.85rem auto auto;
                z-index: 3;
            }

            .the-gas-station-header__bottom-lines {
                position: absolute;
                width: 100%;
                bottom: 0;

                .the-gas-station-header__white-line {
                    background: $white;
                    height: 0.1875rem;
                }

                .the-gas-station-header__red-line {
                    background: $red;
                    height: 0.5rem;
                }

            }
        }


        .the-gas-station__content {
            background: $bg-content;
            padding: 3.25rem 1.4375rem 0.3125rem;
            border-bottom-left-radius: 0.625rem;
            border-bottom-right-radius: 0.625rem;


            & > * {
                margin-bottom: 0.375rem;
            }

            .the-gas-station-content__title {
                font-size: 1.5rem;
                color: $white;
                text-align: left;
            }

            .the-gas-station-content__petrol-type {
                display: flex;
                cursor: pointer;

                .the-gas-station-calc-petrol-calc__logo {
                    display: flex;
                    align-items: center;
                    justify-content: center;
                    border-radius: 0.8125rem;
                    border: 0.3125rem solid #bdc3c7;
                    width: 3.75rem;
                    height: 3.75rem;
                    transform: scale(-1, 1);

                    img {
                        width: 2.8125rem;
                        height: 2.8125rem;
                    }
                }
            }
        }
    }
</style>
