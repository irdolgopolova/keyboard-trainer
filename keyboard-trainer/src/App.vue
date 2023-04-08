<template>
    <header class="header">
        <div class="container">
            <h2 class="header__title">Тренировочное задание</h2>

            <div class="header__config">
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


                <div class="header__focus">
                    <input
                        class="header__focus__input"
                        type="checkbox"
                        id="focus"
                        v-model="focusMode"
                        @change="changeFocusMode"
                    />
                    <label
                        class="header__focus__label"
                        for="focus"
                    >
                        Фокус
                    </label>
                </div>
            </div>
        </div>
    </header>
    <main :class="[
            'content',
            {'blur' : endTask }
        ]"
    >
        <div class="container">

            <div
                v-if="endTask"
                class="ending-modal"
            >
                <header class="ending-modal__header">
                    <h2 class="ending-modal__header__title">
                        Вы завершили задание
                    </h2>
                </header>
                <main class="ending-modal__body">
                    <p class="ending-modal__body__stitistics-title">
                        Количество символов
                        <span
                            class="ending-modal__body__stitistics-title__value"
                        >
                            {{ totalLetters }}
                        </span>
                    </p>
                    <p class="ending-modal__body__stitistics-title">
                        Средняя скорость
                        <span
                            class="ending-modal__body__stitistics-title__value"
                        >
                            {{ averageSpeed }}
                        </span>
                    </p>
                    <p class="ending-modal__body__stitistics-title">
                        Ошибок
                        <span
                            class="ending-modal__body__stitistics-title__value"
                        >
                            {{ errorsCount }}
                        </span>
                    </p>
                    <button
                        class="ending-modal__body__btn"
                        @click="startNextTask"
                    >
                        Следующее задание
                    </button>
                </main>
            </div>

            <div :class="[
                    'task-box',
                    {'task-box__error' :  error}
                ]"
            >
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

            <div
                v-if="focusMode === false"
                class="interactive-box"
            >
                <div class="interactive-box__hand">
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
                                {'active' : isCurrent(index, key)},
                                {'error' : isCurrent(index, key) && error }
                            ]"
                        >
                            {{ index }}
                        </div>
                    </div>
                </div>

                <div class="interactive-box__hand">
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
        <div class="container">
            <h2 class="title">Мастер класс Клавиатурный тренажер на Vue.js</h2>
        </div>
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
        error: false,
        errorsCount: 0,
        successCount: 0,
        focusMode: false,
        endTask: false,
    }
  },
  computed: {
    isLeftEmpty() {
        return this.currentFinger ? this.currentFinger.match(/right/i) : true
    },
    isRightEmpty() {
        return this.currentFinger ? this.currentFinger.match(/left/i) : true
    },
    successSpeed() {
        if (this.successCount === 0) return 0

        if (this.successCount === 1) {
            sessionStorage.setItem('startTime', new Date().getTime())
        }

        let startTime = +sessionStorage.getItem('startTime')
        let currentTime = new Date().getTime()
        let leftTimeinMin = ((currentTime - startTime) / 60000).toFixed(2)

        return Math.floor(this.successCount / (leftTimeinMin === 0 ? 1 : leftTimeinMin))
    },
    totalLetters() {
        return this.correctlyLetters.length
    },
    averageSpeed() {
        let startTime = +sessionStorage.getItem('startTime')
        let currentTime = new Date().getTime()
        let leftTimeinMin = ((currentTime - startTime) / 60000).toFixed(2)

        return Math.floor(this.successCount / (leftTimeinMin === 0 ? 1 : leftTimeinMin))
    },
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
    pressKey(e) {
        if (e.key === this.currentLetter) {
            this.successCount++

            this.correctlyLetters.push(this.currentLetter)

            if (this.letters.length > 0) {
                this.currentLetter = this.letters.shift()
            } else {
                this.endTask = true
            }
        } else if (
            this.error === false
            && this.successCount > 0
        ) {
            this.error = true
            this.errorsCount++

            setTimeout(() => this.error = false, 500)
        }
    },
    isCurrent(key, keyData) {
        let keys = (key === 'space') ? [' '] : key.split(' ')

        if (keys[0] === 'space') {
            console.log('keys[0]', keys[0])
        }

        let isCurrentKey = keys.includes(this.currentLetter.toLowerCase())

        if (isCurrentKey) {
            this.currentFinger = `${keyData.hand}_${keyData.finger}`
        }

        return isCurrentKey
    },
    changeFocusMode(event) {
        event.target.blur()
    },
    startNextTask() {
        this.getTextData()

        sessionStorage.removeItem('startTime')

        this.correctlyLetters = []
        this.errorsCount = 0
        this.successCount = 0

        this.endTask = false
    },
  },
  created() {
    this.getTextData()
  },
  mounted() {
    document.addEventListener('keyup', this.pressKey)
  }
}
</script>

<style lang="scss" scoped>
@import './assets/base.scss';

</style>
