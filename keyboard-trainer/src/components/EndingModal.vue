<template>
    <div class="ending-modal">
        <header class="ending-modal__header">
            <h2 class="ending-modal__header__title">
                Вы завершили задание
            </h2>
        </header>
        <main class="ending-modal__body">
            <p
                v-for="(value, name) in endingData" :key="name"
                class="ending-modal__body__stitistics-title"
            >
                {{name}}
                <span
                    class="ending-modal__body__stitistics-title__value"
                >
                    {{value}}
                </span>
            </p>

            <!-- для кнопки нужны стили -->
            <button
                class="ending-modal__body__btn"
                @click="startNextTask"
            >
                Следующее задание
            </button>
        </main>
    </div>
</template>

<script>
export default {
    name: "EndingModal",
    props: {
        endingData: {
            type: Object,
            default: {
                "Количество символов": 0,
                "Средняя скорость": 0,
                "Ошибок": 0,
            }
        }
    },
    methods: {
        startNextTask() {
            this.$parent.setInitialValues()
            this.$parent.getTextData()
        },
    }
}
</script>