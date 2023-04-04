<template>
    <header class="header">
        <div class="container">
            <h2 class="header__title">Тренировочное задание</h2>

            <div class="header__statictic">
                <div class="header__statictic__item">
                    <speedIcon />
                    <span class="header__statictic__item__value">
                        0
                    </span>
                </div>

                <div class="header__statictic__item">
                    <errorsIcon />
                    <span class="header__statictic__item__value">
                        0
                    </span>
                </div>
            </div>
        </div>
    </header>
    <main class="content">
        <div class="container">
            <div class="task-box">
                <p class="task-box__text">
                    Для современного мира новая модель организационной деятельности
                    требует от нас анализа новых предложений. Равным образом, новая
                    модель организационной деятельности является качественно новой
                    ступенью дальнейших направлений развития. В рамках спецификации
                    современных стандартов, явные признаки победы институционализации
                    будут объявлены нарушающими общечеловеческие нормы этики и морали.
                </p>
            </div>

            <div class="interactive-box">
                <div class="interactive-box__left-hand">
                    <leftEmpty />
                </div>

                <div class="interactive-box__keyboard">
                    <div class="interactive-box__keyboard__key">
                        ё
                    </div>
                    <div class="interactive-box__keyboard__key">
                        1
                    </div>
                    <div class="interactive-box__keyboard__key">
                        2
                    </div>
                    <div class="interactive-box__keyboard__key">
                        3
                    </div>
                    <div class="interactive-box__keyboard__key">
                        4
                    </div>
                    <div class="interactive-box__keyboard__key">
                        5
                    </div>
                    <div class="interactive-box__keyboard__key">
                        6
                    </div>
                    <div class="interactive-box__keyboard__key">
                        7
                    </div>
                    <div class="interactive-box__keyboard__key">
                        8
                    </div>
                    <div class="interactive-box__keyboard__key">
                        9
                    </div>
                    <div class="interactive-box__keyboard__key">
                        0
                    </div>
                    <div class="interactive-box__keyboard__key">
                        -
                    </div>
                    <div class="interactive-box__keyboard__key">
                        =
                    </div>
                    <div class="interactive-box__keyboard__key">
                        Backspace
                    </div>
                </div>

                <div class="interactive-box__right-hand">
                    <rightEmpty />
                </div>
            </div>
        </div>
    </main>
    <footer class="footer">
        <h2 class="title">Мастер класс Клавиатурный тренажер на Vue.js</h2>
    </footer>
</template>

<script>
import speedIcon from "./components/icons/speedIcon.vue"
import errorsIcon from "./components/icons/errorsIcon.vue"

import leftEmpty from "./components/icons/hands/leftEmpty.vue"
import rightEmpty from "./components/icons/hands/rightEmpty.vue"

export default {
  components: {
    speedIcon,
    errorsIcon,
    leftEmpty,
    rightEmpty
  }
}
</script>

<style lang="scss" scoped>
@import './assets/base.scss';

.header {
    .container {
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        align-items: center;
    }

    &__title {
        @extend .title;
    }

    &__statictic {
        width: 100%;
        max-width: 200px;

        display: flex;
        flex-direction: row;
        justify-content: space-between;
        align-items: center;

        &__item {
            display: flex;
            flex-direction: row;
            justify-content: space-between;
            align-items: center;

            &:not(:last-child) {
                margin-right: 48px;
            }

            &__value {
                @extend .text;

                margin-left: 16px;
                color: $c-gray;
            }
        }

    }
}

.task-box {
    padding: 24px;
    margin: 80px auto;

    width: 100%;
    max-width: 1000px;

    border: 2px solid $c-gray;
    border-radius: $border-radius;

    &__text {
        @extend .text;
    }
}

.interactive-box {
    margin: 80px auto;

    width: 100%;
    max-width: 1400px;

    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;

    &__keyboard {
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        align-items: center;

        width: 100%;
        max-width: 1000px;

        @media (max-width: $tablet-max-width) {
            max-width: 900px;
        }

        &__key {
            @extend .keyboard__key;

            padding: 2px;
            margin: 8px 4px;

            width: 100%;

            border: 1px solid $c-gray;
            border-radius: $border-radius;
        }
    }
}
</style>
