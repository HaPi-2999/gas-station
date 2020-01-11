<template>
    <div class="the-gas-station-progress-ring">
        <svg width="6.25rem" height="6.25rem">
            <circle stroke="#42505f"
                    class="the-gas-station-progress-ring-circle"
                    fill="transparent"
                    :stroke-width="stroke"
                    :r="normalizedRadius"
            />
            <circle
                    class="the-gas-station-progress-ring-circle"
                    id="progress-ring"
                    stroke="#5D9F23"
                    fill="transparent"
                    :stroke-dasharray="circumference + ' ' + circumference"
                    style="border-radius: 3px;"
                    :style="{ strokeDashoffset }"
                    :stroke-width="stroke"
                    :r="normalizedRadius"
            />
        </svg>
        <div class="the-gas-station-progress-ring-text-in-circle ">{{count_litters + 'Л'}}</div>
    </div>
</template>

<script>
    export default {
        name: "TheGasStationProgressRing",
        props: {
            count_litters: Number,
        },
        data() {
            const radius = 40,
                stroke = 7,
                normalizedRadius = radius - stroke,
                circumference = normalizedRadius * 2 * Math.PI;

            return {
                radius,
                stroke,
                normalizedRadius,
                circumference
            };
        },
        computed: {
            strokeDashoffset() {
                let progress = this.count_litters * 100 / 40;
                return this.circumference - progress / 100 * this.circumference;
            },
        }
    }
</script>

<style lang="scss">
    .the-gas-station-progress-ring-circle {
        transform: rotate(-90deg);
        transform-origin: 2.9375rem 0;
        stroke-linecap: round;
    }

    .the-gas-station-progress-ring {
        position: relative;
        width: 6.25rem;
        height: 6.25rem;

        .the-gas-station-progress-ring-text-in-circle {
            position: absolute;
            top: -0.125rem;
            left: -0.1875rem;
            line-height: 6.25rem;
            text-align: center;
            width: 100%;
            font-size: 1rem;
            font-weight: bold;
        }
    }
</style>
