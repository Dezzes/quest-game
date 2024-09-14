<template>
    <div :class="[$style.Tooltip, {[$style._show]: isShow}]">
        <h4
            v-if="data.title"
            :class="$style.tooltipTitle"
        >
            {{ data.title }}
        </h4>

        <hr v-if="data.desc"/>

        <p
            v-if="data.desc"
            :class="$style.tooltipMessage"
        >
            {{ data.desc }}
        </p>

        <slot/>
    </div>
</template>

<script>
export default {
    name: 'Tooltip',

    props: {
        data: {
            type: Object,
            default: () => ({}),
        },

        isShow: {
            type: Boolean,
            default: false,
        }
    },
}
</script>

<style lang="scss" module>
    .Tooltip {
        position: absolute;
        z-index: 100;
        width: 364px;
        padding: 16px 16px 16px 45px;
        border: 2px solid $dark-orange;
        border-radius: 8px;
        background-color: rgba(9, 22, 38, 0.95);
        opacity: 0;
        pointer-events: none;
        transition: .2s ease-in-out opacity;

        &._show {
            opacity: 1;
        }
    }

    .tooltipTitle {
        font-size: 16px;
        font-weight: 600;
        line-height: 19.36px;
        color: $white;
    }

    .tooltipMessage {
        font-size: 14px;
        line-height: 19px;
        letter-spacing: 0.01em;
        color: $white;
    }
</style>
