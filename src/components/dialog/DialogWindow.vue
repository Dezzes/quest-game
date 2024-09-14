<template>
    <div :class="$style.DialogWindow">
        <img
            src="@/assets/img/character.png"
            alt="character"
            :class="$style.character"
        />

        <div :class="$style.dialog">
            <header :class="$style.header">
                <h4 :class="$style.title">
                    {{ data.characterName }}
                </h4>

                <p :class="$style.desc">
                    {{ data.question }}
                </p>
            </header>

            <div :class="$style.content">
                <DialogItem
                    v-for="(answer, idx) in data.answers"
                    :key="`${idx}-DialogItem`"
                    :item="answer"
                    @click="handleUserAnswer(answer)"
                />
            </div>
        </div>
    </div>
</template>

<script>
import DialogItem from '@/components/dialog/DialogItem.vue';

const characterName = 'Имя персонажа';
const desc = 'Теперь образование - это не скучные лекции, на которых хочется спать, а неизведанный мир, в который каждый захочет погрузиться с головой. Есть запасное место для написание еще одного предложения внутри диалога';

export default {
    name: 'DialogWindow',

    components: {DialogItem},

    props: {
        data: {
            type: Object,
            default: () => ({}),
        },
    },

    data() {
        return {
            characterName,
            desc,
        }
    },

    methods: {
        handleUserAnswer(userAnswer) {
            if (this.data.correctAnswer === userAnswer) {
                this.$emit('answer', this.data);
            }
        }
    }
}
</script>

<style lang="scss" module>
    .DialogWindow {
        position: absolute;
        bottom: 0;
        z-index: 100;
        display: flex;
        align-items: center;
        width: 100%;
    }

    .character {
        max-width: 200px;
        height: 100%;
    }

    .dialog {
        width: 100%;
        margin: 0 8px 12px;
    }

    .header {
        background-color: rgba(9, 22, 38, .95);
        padding: 3px 3px 0;
        border: 2px solid $orange;
        border-radius: 6px;
        user-select: none;
        box-shadow: 0 -2px 0 0 $orange-100 inset;
    }

    .title {
        padding: 8px 12px;
        border-radius: 6px 6px 0 0;
        background: linear-gradient(0deg, #FC7C1E 0%, #FFAD23 100%);
        font-size: 16px;
        font-weight: 600;
        color: $white;
    }

    .desc {
        padding: 7px 16px 30px;
        color: $white;
    }

    .content {
        display: grid;
        grid-template-columns: repeat(2, 1fr);
        grid-template-rows: repeat(2, 1fr);
        margin-top: 5px;
        gap: 5px;
    }
</style>
