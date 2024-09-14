<template>
    <div :class="$style.app">
        <GameScene ref="gameScene">
            <transition :name="currentItemData ? 'fade' : 'slide-up'">
                <DialogWindow
                    v-if="currentItemData"
                    :key="`${currentItemData.id}-DialogWindow`"
                    :data="currentItemData"
                    @answer="onUserCorrectAnswer"
                />
            </transition>

            <GameItem
                :ref="`gameItem-${item.id}`"
                v-for="item in gameData"
                :key="item.id"
                :style="ItemPosition(item)"
                @click="onQuestItemClick(item)"
                @mouseenter="handleHover(item, $event)"
                @mouseleave="handleLeave(item)"
            >
                <img
                    src="/src/assets/img/wood.png"
                    alt="wood"
                    :class="$style.wood"
                />
            </GameItem>

            <Tooltip
                ref="tooltip"
                :data="tooltipData"
                :is-show="Boolean(hoveredItem)"
                :style="position"
            >
                <img
                    src="@/assets/img/tooltip-img.png"
                    alt="tooltip-img"
                    :class="$style.tooltipImg"
                />
            </Tooltip>
        </GameScene>
    </div>
</template>

<script>
//Libraries
import { gsap } from "gsap";

//Components
import DialogWindow from '@/components/dialog/DialogWindow.vue';
import GameItem from '@/components/GameItem.vue';
import Tooltip from '@/components/dialog/Tooltip.vue';
import GameScene from '@/components/Layouts/GameScene.vue';

//Constants
import { gameData } from '@/assets/constants/gameData.js';
//Constants

const tooltipData = {
    title: 'Доска',
    desc: 'Она очень деревянная и лежит прямо тут. Множество досок образуют мост, что поможет попасть на другую сторону',
}

export default {
    name: 'App',

    components: {
        GameScene,
        Tooltip,
        GameItem,
        DialogWindow
    },

    data() {
        return {
            hoveredItem: null,
            showDialog: false,
            currentItemData: null,

            gameData,
            tooltipData,

            tooltipHeight: 0,
            tooltipWidth: 0,
            top: 0,
            left: 0,
        }
    },

    mounted() {
        this.tooltipHeight = this.$refs.tooltip.$el?.offsetHeight || 0;
        this.tooltipWidth = this.$refs.tooltip.$el?.offsetWidth + 50 || 0;
    },

    computed: {
        // Для перемещения тултипа
        position() {
            const cursorOffset = {
                x: 0,
                y: 0,
            };

            const containerOffset = {
                x: 24,
                y: 24,
            };

            const tooltipHeight = this.tooltipHeight;
            const tooltipWidth = this.tooltipWidth;

            const pointerX = this.left;
            const pointerY = this.top;

            const visualWidth = this.$refs.gameScene?.$el?.getBoundingClientRect()?.width || 0;


            let x;
            let y;

            // Если тултип не влезает по высоте сверху
            if (pointerY + cursorOffset.y - tooltipHeight < containerOffset.y) {
                y = pointerY + cursorOffset.y + containerOffset.y * 2;
                // Если все ок
            } else {
                y = pointerY + cursorOffset.y;
            }

            // Если тултип не влезает по ширине справа
            if (pointerX + containerOffset.x + cursorOffset.x + tooltipWidth > visualWidth) {
                x = pointerX - tooltipWidth - cursorOffset.x;

                // Если тултип не влезает по ширине слева
            } else if (pointerX + cursorOffset.x < tooltipWidth) {
                x = pointerX + cursorOffset.x;

                // Если все ок
            } else {
                // x = pointerX + cursorOffset.x - visualOffsetX;
                x = pointerX - tooltipWidth - cursorOffset.x;
            }

            x = x < 0 ? 0 : x;
            y = y < 0 ? 0 : y;

            return {
                top: y + 'px',
                left: x + 'px',
            };
        },
    },

    methods: {
        ItemPosition(gameObjectData) {
            return gameObjectData.finished ? {
                top: `${gameObjectData.finalPosition?.top}px`,
                left: `${gameObjectData.finalPosition?.left}px`,
            } :
                {
                    top: `${gameObjectData.initialPosition?.top}px`,
                    left: `${gameObjectData.initialPosition?.left}px`,
                }

        },

        onQuestItemClick(item) {
            if (item.finished) {
                return;
            }

            this.toggleDialogData(item);
        },

        toggleDialogData(item) {
            if (this.currentItemData === item) {
                this.currentItemData = null;
            } else {
                this.currentItemData = item;
            }
        },

        animateItem(item) {
            this.currentItemData = null;

            gsap.to(this.$refs[`gameItem-${item.id}`]?.[0]?.$el, {
                top: `${item.finalPosition?.top}px`,
                left: `${item.finalPosition?.left}px`,
                pointerEvents: 'none',
                width: 187,
                height: 133,
                zIndex: 1000,
                duration: 2, // Длительность анимации в секундах
                ease: "power2.out", // Плавная анимация в конце
                onComplete: () => this.onCompleteAnimation(item),
            });
        },

        onCompleteAnimation(gameObjectData) {
            const index = this.gameData.findIndex(item => item.id === gameObjectData.id);

            if (index !== -1) {
                this.gameData[index].finished = true;
            }

            const el = this.$refs[`gameItem-${gameObjectData.id}`][0]?.$el;

            if(el) {
                el.style.animation = 'none';
                el.style.zIndex = 2;
            }
        },

        onUserCorrectAnswer(gameObjectData) {
            this.animateItem(gameObjectData);

            this.showDialog = false;
        },

        handleHover(item, evt) {
            if(item.finished || this.showDialog) {
                return;
            }

            this.top = evt.pageY;
            this.left = evt.pageX;

            this.hoveredItem = item;
        },

        handleLeave(item) {
            if(item.finished || this.showDialog) {
                return;
            }

            this.hoveredItem = null;
        }
    }
}
</script>

<style lang="scss" module>
    .app {
        min-height: 100vh;
        width: 100%;
    }

    .tooltipImg {
        position: absolute;
        top: -14px;
        left: -50px;
        width: 75px;
        height: 75px;
        border-radius: 12px;
        border: 2px solid $dark-blue;
        box-shadow: 0 0 4px 2px $orange;
    }

    .wood {
        width: 100%;
        height: 100%;
    }
</style>
