<template>
    <div class="header__focus">
        <input
            class="header__focus__input"
            type="checkbox"
            id="focus"
            value="focusMode"
            @change="changeFocusMode"
        />
        <label
            class="header__focus__label"
            for="focus"
        >
            Фокус
        </label>
    </div>
</template>

<script>

export default {
    name: "FocusMode",
    props: {
        focusMode: {
            type: Boolean,
            default: false
        }
    },
    emits: ['update:focusMode'],
    methods: {
        changeFocusMode(event) {
            event.target.blur()
            this.$parent.$emit('update:focusMode', event.target.checked)
        }
    },
}
</script>