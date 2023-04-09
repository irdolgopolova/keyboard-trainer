<template>
    <Header
        :successCount="successCount"
        :errorsCount="errorsCount"
        v-model:focusMode="focusMode"
    />

    <main :class="[
            'content',
            {'blur' : endTask }
        ]"
    >
        <div class="container">
            <EndingModal
                v-if="endTask"
                :endingData="getEndingData()"
            />

            <TaskBox
                :textData="textData"
                :error="error"
            />

            <InteractiveBox
                v-if="focusMode === false"
                v-model:successCount="successCount"
                v-model:letters="letters"
                v-model:correctlyLetters="correctlyLetters"
                v-model:currentLetter="currentLetter"
                v-model:errorsCount="errorsCount"
                v-model:error="error"
                v-model:endTask="endTask"
            />
        </div>
    </main>

    <Footer />
</template>

<script>
import Header from "./components/Header.vue"

import EndingModal from "./components/EndingModal.vue"
import TaskBox from "./components/TaskBox.vue"
import InteractiveBox from "./components/InteractiveBox.vue"

import Footer from "./components/Footer.vue"

export default {
    components: {
        Header,
        EndingModal,
        TaskBox,
        InteractiveBox,
        Footer,
    },
    data() {
        return {
            correctlyLetters: [],
            letters: [],
            currentLetter: "",
            error: false,
            errorsCount: 0,
            successCount: 0,
            focusMode: false,
            endTask: false,
        }
    },
    computed: {
        totalLetters() {
            return this.correctlyLetters.length
        },
        averageSpeed() {
            let startTime = +sessionStorage.getItem('startTime')
            let currentTime = new Date().getTime()
            let leftTimeinMin = ((currentTime - startTime) / 60000).toFixed(2)

            return Math.floor(this.successCount / (leftTimeinMin === "0.00" ? 1 : leftTimeinMin))
        },
        textData() {
            return {
                typedText: this.correctlyLetters.join(''),
                currentLetter: this.currentLetter,
                remainingText: this.letters.join('')
            }
        },
    },
    methods: {
        setInitialValues() {
            sessionStorage.removeItem('startTime')

            this.correctlyLetters = []
            this.errorsCount = 0
            this.successCount = 0

            this.endTask = false
        },
        getTextData() {
            fetch("https://fish-text.ru/get?number=1")
                .then(responce => responce.json())
                .then(data => {
                    if (data.status == "success") {
                        this.letters = data.text.split('')
                        this.currentLetter = this.letters.shift()
                    } else {
                        console.error('error', data.status)
                    }
                })
                .catch(error => console.error('error', error))
        },
        getEndingData() {
            return {
                "Количество символов": this.totalLetters,
                "Средняя скорость": this.averageSpeed,
                "Ошибок": this.errorsCount,
            }
        },
    },
    created() {
        this.getTextData()
    },
}
</script>

<style lang="scss" scoped>
@import './assets/base.scss';

</style>
