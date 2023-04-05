<template>
    <header class="header">
        <div class="container">
            <h2 class="header__title">Тренировочное задание</h2>

            <div class="header__config">
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

                <div class="header__focus">
                    <label class="header__focus__label">
                        <input
                            class="header__focus__input"
                            type="checkbox"
                        />
                        Фокус
                    </label>
                </div>
            </div>
        </div>
    </header>
    <main class="content">
        <div class="container">
            <div class="task-box">
                <p class="task-box__text">
                    {{ text }}
                </p>
            </div>

            <div class="interactive-box">
                <div class="interactive-box__left-hand">
                    <leftEmpty />
                </div>

                <div class="interactive-box__keyboard">
                    <div
                        v-for="(keys, index) in arrayKey" :key="index"
                        class="interactive-box__keyboard__row"
                    >
                        <div
                            v-for="(key, index) in keys" :key="index"
                            class="interactive-box__keyboard__key"
                        >
                            {{ index }}
                        </div>
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

import arrayKey from "./components/keyboard/listKeys.json"

export default {
  components: {
    speedIcon,
    errorsIcon,
    leftEmpty,
    rightEmpty
  },
  data() {
    return {
        text: "",
        arrayKey: arrayKey,
    }
  },
  methods: {
    getTextData() {
        fetch("https://fish-text.ru/get", {
            type:"sentence",
            number:1,
            format:"json"
        })
            .then(responce => responce.json())
            .then(data => {
                if (data.status == "success") {
                    this.text = data.text
                } else {
                    console.log('error', data.status)
                }
            })
            .catch(error => console.log('error', error))

    }
  },
  created() {
    this.getTextData()
  },
  mounted() {
    console.log('arrayKey', arrayKey)
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

    &__config {
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        align-items: center;
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

    &__focus {
        width: 100%;
        max-width: 150px;

        margin: 0 0 0 52px;

        &__label {
            @extend .text;
            color: $c-gray;
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
        flex-direction: column;
        align-items: center;

        width: 100%;
        max-width: 1000px;

        @media (max-width: $tablet-max-width) {
            max-width: 900px;
        }

        &__row {
            display: flex;
            flex-direction: row;
            justify-content: space-between;

            width: 100%;
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
