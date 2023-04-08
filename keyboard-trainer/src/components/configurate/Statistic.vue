<template>
    <div class="header__statictic">
        <div class="header__statictic__item">
            <speedIcon />
            <span class="header__statictic__item__value">
                {{ successSpeed }}
                <span
                    class="header__statictic__item__value__decription"
                >
                    знак/мин
                </span>
            </span>
        </div>

        <div class="header__statictic__item">
            <errorsIcon />
            <span class="header__statictic__item__value">
                {{ errorsCount }}
            </span>
        </div>
    </div>
</template>

<script>
import speedIcon from "./../icons/speedIcon.vue"
import errorsIcon from "./../icons/errorsIcon.vue"

export default {
    name: "Statistic",
    components: {
        speedIcon,
        errorsIcon,
    },
    props: {
        successCount: {
            type: Number,
            default: 0
        },
        errorsCount: {
            type: Number,
            default: 0
        },
    },
    computed: {
        successSpeed() {
            if (this.successCount === 0) return 0

            if (this.successCount === 1) {
                sessionStorage.setItem('startTime', new Date().getTime())
            }

            let startTime = +sessionStorage.getItem('startTime')
            let currentTime = new Date().getTime()
            let leftTimeinMin = ((currentTime - startTime) / 60000).toFixed(2)

            return Math.floor(this.successCount / (leftTimeinMin === "0.00" ? 1 : leftTimeinMin))
        },
    },
}
</script>