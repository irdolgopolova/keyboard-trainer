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
                    <span class="task-box__text__correctly">
                       {{ correctlyLetters.join('') }}
                    </span>
                    <span class="task-box__text__current">
                        {{ currentLetter }}
                    </span>
                    <span>
                       {{ letters.join('') }}
                    </span>
                </p>
            </div>

            <div class="interactive-box">
                <div class="interactive-box__left-hand">
                    <leftEmpty v-if="isLeftEmpty" />
                    <leftLittle v-if="currentFinger === 'left_5'"/>
                    <leftRing v-if="currentFinger === 'left_4'"/>
                    <leftMiddle v-if="currentFinger === 'left_3'"/>
                    <leftForefinger v-if="currentFinger === 'left_2'"/>
                    <leftThumb v-if="currentFinger === 'left_1'"/>
                </div>

                <div class="interactive-box__keyboard">
                    <div
                        v-for="(keys, index) in arrayKey" :key="index"
                        class="interactive-box__keyboard__row"
                    >
                    <!-- вынести в функции -->
                        <div
                            v-for="(key, index) in keys" :key="index"
                            :class="[
                                'interactive-box__keyboard__key',
                                {'space' : index === 'space'},
                                {'pink' : (key.hand === 'left' && key.finger === 5) || (key.hand === 'right' && key.finger === 2)},
                                {'yellow' : (key.hand === 'left' && key.finger === 4) || (key.hand === 'right' && key.finger === 3)},
                                {'green' : (key.hand === 'left' && key.finger === 3) || (key.hand === 'right' && key.finger === 4)},
                                {'blue' : (key.hand === 'left' && key.finger === 2) || (key.hand === 'right' && key.finger === 5)},
                                {'brown' : (key.hand === 'left' && key.finger === 1) || (key.hand === 'right' && key.finger === 1)},
                                {'active' : isCurrent(index, key)}
                            ]"
                        >
                            {{ index }}
                        </div>
                    </div>
                </div>

                <div class="interactive-box__right-hand">
                    <rightEmpty v-if="isRightEmpty"/>
                    <rightLittle v-if="currentFinger === 'right_5'"/>
                    <rightRing v-if="currentFinger === 'right_4'"/>
                    <rightMiddle v-if="currentFinger === 'right_3'"/>
                    <rightForefinger v-if="currentFinger === 'right_2'"/>
                    <rightThumb v-if="currentFinger === 'right_1'"/>
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

import leftLittle from "./components/icons/hands/leftLittle.vue"
import leftRing from "./components/icons/hands/leftRing.vue"
import leftMiddle from "./components/icons/hands/leftMiddle.vue"
import leftForefinger from "./components/icons/hands/leftForefinger.vue"
import leftThumb from "./components/icons/hands/leftThumb.vue"

import rightLittle from "./components/icons/hands/rightLittle.vue"
import rightRing from "./components/icons/hands/rightRing.vue"
import rightMiddle from "./components/icons/hands/rightMiddle.vue"
import rightForefinger from "./components/icons/hands/rightForefinger.vue"
import rightThumb from "./components/icons/hands/rightThumb.vue"

import arrayKey from "./components/keyboard/listKeys.json"

export default {
  components: {
    speedIcon,
    errorsIcon,
    leftEmpty,
    rightEmpty,
    leftLittle,
    leftRing,
    leftMiddle,
    leftForefinger,
    leftThumb,
    rightLittle,
    rightRing,
    rightMiddle,
    rightForefinger,
    rightThumb
  },
  data() {
    return {
        correctlyLetters: [],
        letters: [],
        currentLetter: '',
        currentFinger: 0,
        arrayKey: arrayKey,
        fingers: {
            5: 'Litle',
            4: 'Litle',
            3: 'Litle',
            2: 'Litle',
            1: 'Litle',
        }
    }
  },
  computed: {
    isLeftEmpty() {
        return this.currentFinger.match(/right/i);
    },
    isRightEmpty() {
        return this.currentFinger.match(/left/i);
    }
  },
  methods: {
    getTextData() {
        fetch("https://fish-text.ru/get?number=1")
            .then(responce => responce.json())
            .then(data => {
                if (data.status == "success") {
                    // Разделили полученую строку на символы
                    this.letters = data.text.split('')
                    // выбираем первый элемент массива
                    this.currentLetter = this.letters.shift()
                } else {
                    console.log('error', data.status)
                }
            })
            .catch(error => console.log('error', error))
    },
    test(e) {
        if (e.key === this.currentLetter) {
            this.correctlyLetters.push(this.currentLetter)
            this.currentLetter = this.letters.shift()
        }
        // Добавить обработку неправильного нажатия
    },
    isCurrent(key, keyData) {
        if (key === 'space') {
            key = ' '
        }

        let isCurrentKey = (key === this.currentLetter.toLowerCase())

        if (isCurrentKey) {
            this.currentFinger = `${keyData.hand}_${keyData.finger}`
        }

        return isCurrentKey
    }
  },
  created() {
    this.getTextData()
  },
  mounted() {
    document.addEventListener('keyup', this.test)
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

        &__correctly {
            @extend .text__correctly;
        }

        &__current {
            @extend .text__current;
        }
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
            justify-content: center;

            width: 100%;
        }

        &__key {
            @extend .keyboard__key;

            padding: 2px;
            margin: 8px 4px;

            width: 100%;
            max-width: 80px;

            border: 1px solid $c-gray;
            border-radius: $border-radius;
        }

        .space {
            max-width: 520px;
        }

        .pink {
            background-color: $c-pink;
        }

        .yellow {
            background-color: $c-yellow;
        }

        .brown {
            background-color: $c-brown;
        }

        .green {
            background-color: $c-green;
        }

        .blue {
            background-color: $c-blue;
        }

        .active {
            outline: 3px solid $c-red;
        }
    }
}
</style>
