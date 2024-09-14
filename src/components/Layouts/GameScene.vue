<template>
    <div
        :class="$style.GameScene"
         class="container"
    >
        <img
            src="/src/assets/img/scene.png"
            alt="scene"
            :class="$style.scene"
        />

<!--        <img-->
<!--            src="/src/assets/img/columns.png"-->
<!--            alt="columns"-->
<!--            :class="[$style.column, $style._left]"-->
<!--        />-->

<!--        <img-->
<!--            src="/src/assets/img/columns.png"-->
<!--            alt="columns"-->
<!--            :class="[$style.column, $style._right]"-->
<!--        />-->

        <img
            src="/src/assets/img/columns.png"
            alt="columns"
            :class="[$style.column, $style._left]"
        />

        <img
            src="/src/assets/img/bridge.png"
            alt="bridge"
            :class="$style.bridge"
        />

        <slot/>
    </div>
</template>

<script>
    export default {
        name: 'GameScene'
    }
</script>

<style lang="scss" module>
    .GameScene {
        position: relative;
        width: 1151px;
        min-width: 1151px;
        height: 987px;
        overflow-x: auto;
        overflow-y: hidden;
    }

    .scene {
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        z-index: 1;
        width: 100%;
        height: 100%;
        user-select: none;
        pointer-events: none;
    }

    .column {
        position: absolute;
        width: 100%;
        z-index: 2;
        left: 0;
        top: 0;
        pointer-events: none;
        user-select: none;

        &._left {
          z-index: 2;
        }

        &._right {
            z-index: 6;
            top: 10%;
            left: 10%;
        }
    }

    .bridge {
        position: absolute;
        z-index: 3;
        bottom: 0;
        width: 100%;
    }
</style>
