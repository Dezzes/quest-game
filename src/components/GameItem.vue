<template>
    <div :class="$style.GameItem">
        <img
            src="/src/assets/img/wood.png"
            alt="wood"
        />

        <slot/>
    </div>
</template>

<script>
export default {
    name: 'GameItem',

    props: {
        item: {
            type: Object,
            default: () => ({}),
        },
    },
}
</script>

<style lang="scss" module>
    .GameItem {
        position: absolute;
        z-index: 5;
        cursor: pointer;
        animation: float 4s ease-in-out infinite;
    }

    @keyframes float {
        0% {
            transform: translateY(0);
        }
        50% {
            transform: translateY(-20px);
        }
        100% {
            transform: translateY(0);
        }
    }

</style>
