<template>
    <div :class="$style.Tooltip">
        <h4 :class="$style.tooltipTitle">
            {{ tooltipTitle }}
        </h4>

        <hr/>

        <p :class="$style.tooltipMessage">
            {{ tooltipDesc }}
        </p>
    </div>
</template>

<script>
//Constants
const tooltipTitle = 'Доска';
const tooltipDesc = 'Она очень деревянная и лежит прямо тут. Множество досок образуют мост, что поможет попасть на другую сторону';

export default {
    name: 'Tooltip',

    data() {
        return {
            tooltipTitle,
            tooltipDesc,
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

        &:before {
            content: "";
            position: absolute;
            top: -14px;
            left: -50px;
            width: 75px;
            height: 75px;
            border-radius: 12px;
            border: 2px solid $dark-blue;
            background: url("/src/assets/img/tooltip-img.png");
            background-size: cover;
            background-repeat: no-repeat;
            box-shadow: 0 0 4px 2px $orange;
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
