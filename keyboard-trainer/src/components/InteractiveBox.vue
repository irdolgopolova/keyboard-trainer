<template>
    <div class="interactive-box">
        <Hand
            type="left"
            :currentHand="currentHand"
            :currentFinger="currentFinger"
        />

        <Keyboard
            :error="$parent.error"
            :currentLetter="$parent.currentLetter"
        />

        <Hand
            type="right"
            :currentHand="currentHand"
            :currentFinger="currentFinger"
        />
    </div>
</template>

<script>
import Hand from "./Hand.vue"
import Keyboard from "./keyboard/Keyboard.vue"

export default {
    name: "InteractiveBox",
    components: {
        Hand,
        Keyboard
    },
    data() {
        return {
            currentHand: "left",
            currentFinger: 0,
        }
    },
    methods: {
        pressKey(e) {
            if (e.key === 'Shift') return

            if (e.key === this.$parent.currentLetter) {
                this.$parent.successCount++

                this.$parent.correctlyLetters.push(this.$parent.currentLetter)

                if (this.$parent.letters.length > 0) {
                    this.$parent.currentLetter = this.$parent.letters.shift()
                } else {
                    this.$parent.currentLetter = ''
                    this.$parent.endTask = true
                }
            } else if (
                this.$parent.error === false
                && this.$parent.successCount > 0
            ) {
                this.$parent.error = true
                this.$parent.errorsCount++

                setTimeout(() => this.$parent.error = false, 500)
            }
        },
    },
    mounted() {
        document.addEventListener('keyup', this.pressKey)
    }
}
</script>