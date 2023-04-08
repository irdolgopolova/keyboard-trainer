<template>
    <header class="header">
        <div class="container">
            <h2 class="header__title">Тренировочное задание</h2>

            <div class="header__config">
                <Statistic
                    :successCount="successCount"
                    :errorsCount="errorsCount"
                />

                <FocusMode
                    :focusMode="focusMode"
                />
            </div>
        </div>
    </header>
</template>

<script>
import Statistic from "./configurate/Statistic.vue"
import FocusMode from "./configurate/FocusMode.vue"

export default {
    name: "Header",
    components: {
        Statistic,
        FocusMode,
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
        focusMode: {
            type: Boolean,
            default: false
        },
    },
}
</script>
