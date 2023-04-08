<template>
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
                    {'pink' : isThisColor(key.hand, key.finger, 5, 2)},
                    {'yellow' : isThisColor(key.hand, key.finger, 4, 3)},
                    {'green' : isThisColor(key.hand, key.finger, 3, 4)},
                    {'blue' : isThisColor(key.hand, key.finger, 2, 5)},
                    {'brown' : isThisColor(key.hand, key.finger, 1, 1)},
                    {'active' : isCurrent(index, key)},
                    {'error' : isCurrent(index, key) && error }
                ]"
            >
                {{ index }}
            </div>
        </div>
    </div>
</template>

<script>
import arrayKey from "./listKeys.json"

export default {
    name: "Keyboard",
    props: {
        error: {
            type: Boolean,
            default: false
        },
        currentLetter: {
            type: String,
            default: ""
        },
    },
    data() {
        return {
            arrayKey: arrayKey,
        }
    },
    methods: {
        isCurrent(key, keyData) {
            let keys = (key === 'space') ? [' '] : key.split(' ')
            let isCurrentKey = keys.includes(this.currentLetter.toLowerCase())

            if (isCurrentKey) {
                this.$parent.currentFinger = keyData.finger
                this.$parent.currentHand = keyData.hand
            }

            return isCurrentKey
        },
        isThisColor(hand, finger, leftFinger, rightFinger) {
            return (hand === 'left' && finger === leftFinger)
                || (hand === 'right' && finger === rightFinger)
        }
    },
}
</script>

