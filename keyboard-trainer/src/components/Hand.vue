<template>
    <div class="interactive-box__hand">
        <template v-if="type === 'left'">
            <leftEmpty v-if="currentHand !== type" />
            <leftThumb v-if="finger === `${type}_1`"/>
            <leftForefinger v-if="finger === `${type}_2`"/>
            <leftMiddle v-if="finger === `${type}_3`"/>
            <leftRing v-if="finger === `${type}_4`"/>
            <leftLittle v-if="finger === `${type}_5`"/>
        </template>

        <template v-if="type === 'right'">
            <rightEmpty v-if="currentHand !== type" />
            <rightThumb v-if="finger === `${type}_1`"/>
            <rightForefinger v-if="finger === `${type}_2`"/>
            <rightMiddle v-if="finger === `${type}_3`"/>
            <rightRing v-if="finger === `${type}_4`"/>
            <rightLittle v-if="finger === `${type}_5`"/>
        </template>
    </div>
</template>

<script>
import leftEmpty from "./icons/hands/leftEmpty.vue"
import leftThumb from "./icons/hands/leftThumb.vue"
import leftForefinger from "./icons/hands/leftForefinger.vue"
import leftMiddle from "./icons/hands/leftMiddle.vue"
import leftRing from "./icons/hands/leftRing.vue"
import leftLittle from "./icons/hands/leftLittle.vue"

import rightEmpty from "./icons/hands/rightEmpty.vue"
import rightThumb from "./icons/hands/rightThumb.vue"
import rightForefinger from "./icons/hands/rightForefinger.vue"
import rightMiddle from "./icons/hands/rightMiddle.vue"
import rightRing from "./icons/hands/rightRing.vue"
import rightLittle from "./icons/hands/rightLittle.vue"

export default {
    name: "Hand",
    components: {
        leftEmpty,
        leftThumb,
        leftForefinger,
        leftMiddle,
        leftRing,
        leftLittle,
        rightEmpty,
        rightThumb,
        rightForefinger,
        rightMiddle,
        rightRing,
        rightLittle,
    },
    props: {
        type: {
            type: String,
            default: "left"
        },
        currentHand: {
            type: String,
            default: "left"
        },
        currentFinger: {
            type: Number,
            default: 0
        }
    },
    computed: {
        finger() {
            return `${this.currentHand}_${this.currentFinger}`
        }
    }
}
</script>