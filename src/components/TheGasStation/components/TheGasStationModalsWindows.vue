<template>
    <div id="the-gas-station-modals-windows-modal-window" @click="setStopPropagation($event)">
        <input type="text" v-model="money">
        <div class="the-gas-station-modals-windows-modal-window__header">
            ВЫБЕРИТЕ СПОСОБ ОПЛАТЫ:
        </div>
        <div class="the-gas-station-modals-windows-modal-window__content" v-if="competition.type_payment">
            <div class="the-gas-station-modals-windows-modal-window__button the-gas-station-modals-windows-btn-cash"
                 id="btn-cash"
                 @click="setActiveContent">НАЛИЧНЫЙ РАСЧЁТ
            </div>
            <div class="the-gas-station-modals-windows-modal-window__button the-gas-station-modals-windows-btn-bank-card"
                 id="btn-bank-card"
                 @click="setActiveContent">БАНКОВСКАЯ КАРТА
            </div>
        </div>
        <div class="the-gas-station-modals-windows-modal-window__content" v-if="competition.success">
            <img src="../assets/payment-success.png" alt="">
            <p class="the-gas-station-modals-windows-text-success">Транзакция прошла успешно.<br>
                Дождитесь окончания заправки.</p>
        </div>
        <div class="the-gas-station-modals-windows-modal-window__content" v-if="competition.fail">
            <img src="../assets/payment-error.png" alt="">
            <p class="the-gas-station-modals-windows-text-fail">Не достаточно средств на балансе</p>
        </div>
        <div class="the-gas-station-modals-windows-modal-window__footer">
            <img src="../assets/footer-img.png" alt="">
        </div>
    </div>
</template>

<script>
    export default {
        name: "TheGasStationTypePayment",
        props: {
            amount: {
                default: 0,
                type: Number
            }
        },
        data() {
            return {
                competition: {
                    type_payment: true,
                    success: false,
                    fail: false
                },
                money: 1000
            }
        },
        methods: {
            setStopPropagation(event) {
                                                        event.stopPropagation();
            },
            setActiveContent() {
                this.isEnoughMoney();
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
            }
        }
    }
</script>

<style lang="scss">
    $bg-header: linear-gradient(180deg, rgba(34, 64, 98, 1) 0%, rgba(61, 101, 147, 1) 100%);
    $white: #fff;
    $bg: #132437;
    $bg-btn-bank-card: #ebebeb;
    $bg-btn-cash: #1da32d;


    .the-gas-station-modals-windows-btn-bank-card {
        background: $bg-btn-bank-card;
        color: #192736;
    }

    .the-gas-station-modals-windows-btn-cash {
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

            .the-gas-station-modals-windows-text-success {
                color: #fff;
                font-weight: bold;
                margin: 0.625rem 0 0;
                line-height: 1.0625rem;
                font-size: 0.8125rem;
            }

            .the-gas-station-modals-windows-text-fail {
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