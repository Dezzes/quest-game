<template>
    <div
        :class="[$style.DialogItem, {[ $style._invalid]: isWrongAnswerClicked}]"
        @click="handleClick"
    >
        <p :class="$style.text">
            {{ item }}
        </p>
    </div>
</template>

<script>
export default {
    name: 'DialogItem',

    props: {
        item: {
            type: String,
            default: "",
        },

        data: {
            type: Object,
            default: () => ({}),
        },
    },

    data() {
        return {
            isWrongAnswerClicked: false,
        }
    },

    methods: {
        handleClick(userAnswer) {
            this.$emit('user-answer', this.item);

            if (this.data.correctAnswer !== userAnswer) {
                this.isWrongAnswerClicked = true;

                setTimeout(() => {
                    this.isWrongAnswerClicked = false;
                }, 1000)
            }
        }
    }
}
</script>

<style lang="scss" module>
    .DialogItem {
        display: flex;
        align-items: center;
        width: 100%;
        padding: 16px 26px;
        border-radius: 6px;
        border: 3px;
        background-color: rgba(9, 22, 38, 0.95);
        transition: .2s ease-in-out background-color;
        user-select: none;
        cursor: pointer;

        &._invalid {
            background-color: darkred;

            &:hover {
                background-color: darkred;
            }
        }

        &:hover {
            background-color: $orange;

            &:before {
                background-color: $white;
            }

            .text {
                color: $white;
            }
        }

        &:before {
            content: "";
            display: block;
            width: 6px;
            height: 6px;
            margin-right: 12px;
            border-radius: 50%;
            background-color: $grey;
            transition: .2s ease-in-out background-color;
        }
    }

    .text {
        font-size: 14px;
        color: $grey;
        transition: .2s ease-in-out color;
    }
</style>
