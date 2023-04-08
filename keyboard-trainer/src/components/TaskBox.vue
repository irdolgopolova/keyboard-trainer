<template>
    <div :class="[
            'task-box',
            {'task-box__error' :  error}
        ]"
    >
        <p class="task-box__text">
            <span class="task-box__text__correctly">
                {{ textData.typedText }}
            </span>
            <span class="task-box__text__current">
                {{ textData.currentLetter }}
            </span>
            <span>
                {{ textData.remainingText }}
            </span>
        </p>
    </div>
</template>

<script>
export default {
    name: "TaskBox",
    props: {
        textData: {
            type: Object,
            default: {
                typedText: {
                    type: String,
                    default: ""
                },
                currentLetter: {
                    type: String,
                    default: ""
                },
                remainingText: {
                    type: String,
                    default: ""
                },
            }
        },
        error: {
            type: Boolean,
            default: false
        }
    }
}
</script>