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
    props: {
        successCount: Number,
        letters: Array,
        correctlyLetters: Array,
        currentLetter: String,
        errorsCount: Number,
        error: Boolean,
        endTask: Boolean,
    },
    data() {
        return {
            currentHand: "left",
            currentFinger: 0,
        }
    },
    emits: [
        'update:successCount',
        'update:letters',
        'update:correctlyLetters',
        'update:currentLetter',
        'update:errorsCount',
        'update:error',
        'update:endTask',
    ],
    methods: {
        pressKey(e) {
            if (e.key === 'Shift') return

            if (e.key === this.currentLetter) {
                this.$emit('update:successCount', this.successCount + 1)
                this.$emit('update:correctlyLetters', [
                    ...this.correctlyLetters,
                    this.currentLetter
                ])

                if (this.letters.length > 0) {
                    let newArrayLetters = this.letters
                        .filter((letter, index) => index !== 0)

                    this.$emit('update:letters', newArrayLetters)
                    this.$emit('update:currentLetter', this.letters[0])
                } else {
                    this.$emit('update:currentLetter', '')
                    this.$emit('update:endTask', true)
                }
            } else if (this.error === false && this.successCount > 0) {
                this.$emit('update:error', true)
                this.$emit('update:errorsCount', this.errorsCount + 1)

                setTimeout(() => this.$emit('update:error', false), 500)
            }
        },
    },
    mounted() {
        document.addEventListener('keyup', this.pressKey)
    }
}
</script>