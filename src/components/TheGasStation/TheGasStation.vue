<template lang="pug">
    div
        TheGasStationModalsWindows(:amount="amount")
        div(id="the-gas-station-price-calc" class="the-gas-station-price-calc")
            div(class="the-gas-station-price-calc__header")
                img(src="./assets/logo.png" alt="logo")
                div(class="the-gas-station-header-bottom-lines")
                    div(class="the-gas-station-white-header-line")
                    div(class="the-gas-station-red-header-line")
                    div(class="the-gas-station-white-header-line")
            div(class="the-gas-station-price-calc__content")
                div(class="the-gas-station-title") Выберите тип бензина:
                div(class="the-gas-station-petrol-type the-gas-station-calc-petrol-calc"
                     @click="setActive('unleaded')"
                     :class="{'active': active.unleaded}")
                    div(class="the-gas-station-petrol-logo")
                        img(src="./assets/fuel-unleaded.svg" alt="")
                    div(class="the-gas-station-calc-description")
                        div(class="the-gas-station-calc-description__title unleaded") Unleaded 87
                        div(class="the-gas-station-calc-description__text")
                            | Бензин низкого качества, отсутствуют какие
                            | либо присатки.
                            | Достаточно хороший вариант для среднего класса машин.
                div(class="the-gas-station-petrol-type the-gas-station-calc-petrol-calc"
                     @click="setActive('performance_plus')"
                     :class="{'active': active.performance_plus}")
                    div(class="the-gas-station-petrol-logo")
                        img(src="./assets/fuel-perfomance-plus.svg" alt="")
                    div(class="the-gas-station-calc-description")
                        div(class="the-gas-station-calc-description__title performance-plus") Performance Plus
                        div(class="the-gas-station-calc-description__text")
                            | Бензин среднего качества, Добавлена слабая
                            | присадка. Двигатель
                            | становится чуть мощнее, что даёт возможность передвигаться уверенее.
                div(class="the-gas-station-petrol-type the-gas-station-calc-petrol-calc"
                     @click="setActive('performance_premium')"
                     :class="{'active': active.performance_premium}")
                    div(class="the-gas-station-petrol-logo")
                        img(src="./assets/fuel-perfomance-premium.svg" alt="")
                    div(class="the-gas-station-calc-description")
                        div(class="the-gas-station-calc-description__title performance-premium") Performance PREMIUM
                        div(class="the-gas-station-calc-description__text")
                            | Бензин высокого качества, есть особый состав
                            | присадки, который
                            | увеличивает мощность двигателя в разы. Минимальный износ двигателя.
                TheGasStationCalc(
                        :max_liters="40"
                        :min_liters="20"
                        :active_classes="color_and_background"
                        v-on:setAmount="setAmount")
            div(class="the-gas-station-button" id="proceed" :class="color_and_background.background_class"
                 @click="proceed") Продолжить
</template>

<script>
    import TheGasStationCalc from "./components/TheGasStationCalc";
    import TheGasStationModalsWindows from "./components/TheGasStationModalsWindows";

    const CLASSES = [
        'the-gas-station-bg-unleaded',
        'the-gas-station-bg-performance-plus',
        'the-gas-station-bg-performance-premium'
    ];

    const SHADOW_CLASSES = [
        'the-gas-station-bs-unleaded',
        'the-gas-station-bs-performance-plus',
        'the-gas-station-bs-performance-premium'
    ];

    export default {
        name: "TheGasStation",
        components: {
            TheGasStationCalc,
            TheGasStationModalsWindows
        },
        data() {
            return {
                active: {
                    unleaded: true,
                    performance_plus: false,
                    performance_premium: false
                },
                show_modals_window: false,
                color_and_background: {},
                amount: 2
            }
        },
        methods: {
            setActive(key) {
                for (let k in this.active) {
                    this.active[k] = false;
                }

                this.active[key] = true;

                this.color_and_background = this.getColorAndBackground();
                this.setStyles();
            },
            getColorAndBackground() {
                let key = null;
                for (let k in this.active) {
                    if (this.active[k] === true) {
                        key = k;
                    }
                }

                switch (key) {
                    case 'unleaded':
                        return {
                            title: 'Unleaded 87',
                            color_class: 'unleaded',
                            stroke_color: 'the-gas-station-stroke-unleaded',
                            background_class: 'the-gas-station-bg-unleaded',
                            box_shadow_class: 'the-gas-station-bs-unleaded',
                            price_per_liter: 13
                        };
                    case 'performance_plus':
                        return {
                            title: 'Performance Plus',
                            color_class: 'performance-plus',
                            stroke_color: 'the-gas-station-stroke-performance-plus',
                            background_class: 'the-gas-station-bg-performance-plus',
                            box_shadow_class: 'the-gas-station-bs-performance-plus',
                            price_per_liter: 16.3

                        };
                    case 'performance_premium':
                        return {
                            title: 'Performance PREMIUM',
                            color_class: 'performance-premium',
                            stroke_color: 'the-gas-station-stroke-performance-premium',
                            background_class: 'the-gas-station-bg-performance-premium',
                            box_shadow_class: 'the-gas-station-bs-performance-premium',
                            price_per_liter: 21
                        };
                }

                return {}
            },
            setStyles() {
                this.setSliderClasses();
                this.setSpinnerClasses();
            },
            setSpinnerClasses() {
                const SPINNER_CLASSES = [
                    'the-gas-station-stroke-unleaded',
                    'the-gas-station-stroke-performance-plus',
                    'the-gas-station-stroke-performance-premium'
                ];

                let spinner_item = document.getElementById('progress-ring');

                SPINNER_CLASSES.forEach(cl => {
                    if (spinner_item.classList.contains(cl)) {
                        spinner_item.classList.remove(cl);
                    }
                });

                spinner_item.classList.add(this.color_and_background.stroke_color);

            },
            setSliderClasses() {
                const SLIDER_CLASSES = [
                    'range-slider-fill',
                    'range-slider-knob'
                ];

                //Slider_class => sl_cl
                SLIDER_CLASSES.forEach(sl_cl => {
                    let item = document.getElementsByClassName(sl_cl)[0];

                    CLASSES.forEach(cl => {
                        if (item.classList.contains(cl)) {
                            item.classList.remove(cl);
                        }
                    });

                    SHADOW_CLASSES.forEach(cl => {
                        if (item.classList.contains(cl)) {
                            item.classList.remove(cl);
                        }
                    });

                    item.classList.add(this.color_and_background.background_class);
                    item.classList.add(this.color_and_background.box_shadow_class);
                });
            },

            outsideClickListener() {
                let elem = document.getElementById('the-gas-station-modals-windows-modal-window'),
                    _self = this,
                    mainWindow = document.getElementById('the-gas-station-price-calc');

                if (mainWindow.classList.contains('the-gas-station-blur')) {
                    mainWindow.classList.remove('the-gas-station-blur');
                    elem.classList.remove('the-gas-station-modal-active');

                    document.removeEventListener('click', _self.outsideClickListener);
                }
            },

            proceed() {
                let modal = document.getElementById('the-gas-station-modals-windows-modal-window'),
                    mainWindow = document.getElementById('the-gas-station-price-calc'),
                    _self = this,
                    promise = new Promise(function (resolve) {
                        setTimeout(function () {
                            resolve(document.addEventListener('click', _self.outsideClickListener))
                        }, 1)
                    });

                promise.then(function () {
                    mainWindow.classList.add('the-gas-station-blur');
                    modal.classList.add('the-gas-station-modal-active');
                });
            },

            setAmount(amount) {
                this.amount = amount;
            }
        },

        mounted() {
            this.color_and_background = this.getColorAndBackground()
            this.setStyles();
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

    .modal-active {
        background: red;
        z-index: 99;
    }

    .active {
        background: linear-gradient(90deg, rgba(45, 60, 77, 1) 0%, rgba(255, 255, 255, 0) 100%);
        border-radius: 0.625rem;
        padding-left: 0.3125rem !important;
        margin-left: -0.3125rem;
    }

    .the-gas-station-blur {
        filter: blur(3px);
    }

    .the-gas-station-button {
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

    .unleaded {
        color: $unleaded;
    }

    .the-gas-station-bg-unleaded {
        background: $unleaded !important;
    }

    .the-gas-station-stroke-unleaded {
        stroke: $unleaded;
    }

    .the-gas-station-bs-unleaded {
        box-shadow: 0 0 7px 2px rgba(235, 15, 15, 0.66);
    }

    .performance-plus {
        color: $performance-plus;
    }

    .the-gas-station-bg-performance-plus {
        background: $performance-plus !important;
    }

    .the-gas-station-bs-performance-plus {
        box-shadow: 0 0 7px 2px rgba(82, 200, 229, 0.66);
    }

    .the-gas-station-stroke-performance-plus {
        stroke: $performance-plus;
    }

    .performance-premium {
        color: $performance-premium;
    }

    .the-gas-station-bg-performance-premium {
        background: $performance-premium !important;
    }

    .the-gas-station-bs-performance-premium {
        box-shadow: 0 0 7px 2px rgba(183, 237, 81, 0.66);
    }

    .the-gas-station-stroke-performance-premium {
        stroke: $performance-premium;
    }


    .the-gas-station-calc-petrol-calc {
        padding: 0.3125rem 0;

        .the-gas-station-calc-description {
            margin-left: 0.625rem;
            width: 80%;
            text-align: left;

            .the-gas-station-calc-description__title {
                font-size: 1.125rem;
                font-weight: bold;
                margin-top: 0.3125rem;
            }

            .the-gas-station-calc-description__text {
                font-size: 0.8125rem;
                line-height: 0.8125rem;
            }
        }
    }

    .the-gas-station-price-calc {
        width: 100%;
        height: auto;
        color: $white;

        .the-gas-station-price-calc__header {
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

            .the-gas-station-header-bottom-lines {
                position: absolute;
                width: 100%;
                bottom: 0;

                .the-gas-station-white-header-line {
                    background: $white;
                    height: 0.1875rem;
                }

                .the-gas-station-red-header-line {
                    background: $red;
                    height: 0.5rem;
                }

            }
        }


        .the-gas-station-price-calc__content {
            background: $bg-content;
            padding: 3.25rem 1.4375rem 0.3125rem;
            border-bottom-left-radius: 0.625rem;
            border-bottom-right-radius: 0.625rem;


            & > * {
                margin-bottom: 0.375rem;
            }

            .the-gas-station-title {
                font-size: 1.5rem;
                color: $white;
                text-align: left;
            }

            .the-gas-station-petrol-type {
                display: flex;
                cursor: pointer;

                .the-gas-station-petrol-logo {
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
