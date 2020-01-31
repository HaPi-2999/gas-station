<template lang="pug">
    div#the-gas-station-modals-windows-modal-window(
        v-click-outside="vcoConfig"
        :class="{'the-gas-station-modal-active': active_modal}"
    )
        input(type="text" v-model="money")
        div.the-gas-station-modals-windows-modal-window__header ВЫБЕРИТЕ СПОСОБ ОПЛАТЫ:
        div.the-gas-station-modals-windows-modal-window__content(v-if="competition.type_payment")
            div.the-gas-station-modals-windows-modal-window__button.the-gas-station-modals-windows__btn-cash
                #btn-cash(@click.stop="setActiveContent") НАЛИЧНЫЙ РАСЧЁТ
            div.the-gas-station-modals-windows-modal-window__button.the-gas-station-modals-windows__btn-bank-card
                #btn-bank-card(
                    @click.stop="setActiveContent") БАНКОВСКАЯ КАРТА
        div.the-gas-station-modals-windows-modal-window__content(v-if="competition.success")
            img(src="../assets/payment-success.png" alt="")
            p.the-gas-station-modals-windows__text-success Транзакция прошла успешно.
                br
                | Дождитесь окончания заправки.
        div.the-gas-station-modals-windows-modal-window__content(v-if="competition.fail")
            img(src="../assets/payment-error.png" alt="")
            p.the-gas-station-modals-windows__text-fail Не достаточно средств на балансе
        div.the-gas-station-modals-windows-modal-window__footer
            img(src="../assets/footer-img.png" alt="")
</template>

<script>

    import vClickOutside from 'v-click-outside'

    export default {
        name: "TheGasStationTypePayment",
        directives: {
            clickOutside: vClickOutside.directive
        },
        props: {
            amount: {
                default: 0,
                type: Number
            },
            proceedButton: HTMLDivElement,
            active_modal: {
                type: Boolean
            },
        },
        data() {
            return {
                competition: {
                    type_payment: true,
                    success: false,
                    fail: false
                },
                vcoConfig: {
                    handler: this.mClose,
                    middleware: this.middleware,
                    isActive: true
                },
                money: 1000
            }
        },
        methods: {
            setActiveContent() {
                this.isEnoughMoney();
            },
            middleware(event) {
                return event.target !== this.proceedButton;
            },
            mClose() {
                this.$emit('close-modal');
            },
            isEnoughMoney() {
                this.closeAllModal();
                if (this.money >= this.amount) {
                    this.competition.success = true;
                    return true;
                }

                this.competition.fail = true;

                return false;
            },
            closeAllModal() {
                for (let el in this.competition) {
                    this.competition[el] = false;
                }
            },
        },
    }
</script>

<style lang="scss">
    $bg-header: linear-gradient(180deg, rgba(34, 64, 98, 1) 0%, rgba(61, 101, 147, 1) 100%);
    $white: #fff;
    $bg: #132437;
    $bg-btn-bank-card: #ebebeb;
    $bg-btn-cash: #1da32d;


    .the-gas-station-modals-windows__btn-bank-card {
        background: $bg-btn-bank-card;
        color: #192736;
    }

    .the-gas-station-modals-windows__btn-cash {
        color: $white;
        background: $bg-btn-cash;
        margin-bottom: 0.75rem !important;
    }

    #the-gas-station-modals-windows-modal-window {
        width: 100%;
        z-index: 100;
        position: absolute;
        display: none;
        top: 10rem;

        .the-gas-station-modals-windows-modal-window__header {
            position: relative;
            border-top-left-radius: 0.625rem;
            border-top-right-radius: 0.625rem;
            background: $bg-header;
            height: 3.875rem;
            padding-left: 5.4375rem;
            text-align: left;
            line-height: 3.875rem;
            color: $white;
            font-size: 1.0625rem;
            font-weight: bold;

            &:before {
                position: absolute;
                content: url("../assets/logo-ltd.png");
                left: 0.9375rem;
                top: 0.8125rem;
            }
        }

        .the-gas-station-modals-windows-modal-window__content, .the-gas-station-modals-windows-modal-window__footer {
            background: $bg;
        }

        .the-gas-station-modals-windows-modal-window__content {
            padding: 0.9375rem 0.9375rem 0;


            .the-gas-station-modals-windows-modal-window__button {
                cursor: pointer;
                margin: auto;
                width: 11.625rem;
                height: 2.125rem;
                line-height: 2.125rem;
                font-weight: bold;
                font-size: 1rem;
                border-radius: 1.0625rem;
            }

            .the-gas-station-modals-windows__text-success {
                color: #fff;
                font-weight: bold;
                margin: 0.625rem 0 0;
                line-height: 1.0625rem;
                font-size: 0.8125rem;
            }

            .the-gas-station-modals-windows__text-fail {
                color: #bc0000;
                font-weight: bold;
                margin-bottom: 0;
                font-size: 0.8125rem;
            }

        }

        .the-gas-station-modals-windows-modal-window__footer {
            border-bottom-left-radius: 0.625rem;
            border-bottom-right-radius: 0.625rem;
            height: 2.5rem;
            line-height: 3.125rem;
            text-align: center;
            padding-top: 0.25rem;
        }
    }

    .the-gas-station-modal-active {
        display: block !important;
    }
</style>