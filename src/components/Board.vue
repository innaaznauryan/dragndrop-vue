<template>
    <div
            class="board"
            :style="{
                'min-height': `${itemHeight * 6}px`,
                'height': `calc(100% - ${itemHeight}px)`,
                'padding-bottom': `${itemHeight}px`
            }">
        <Item
                v-for="(item, index) in items"
                :key="'item' + index"
                :data-index="index"
                :itemHeight="itemHeight"
                :style="{'background': `linear-gradient(${properties[boardIndex].color}, 90%, darkseagreen)`}"
                :class="{
                    'new': index === newItem.itemIndex && boardIndex === newItem.boardIndex,
                    'transparent': index === draggableItemIndex && boardIndex === sourceBoardIndex,
                    'transform':
                        target.itemIndex !== null
                        && index >= target.itemIndex
                        && boardIndex === target.boardIndex,
                }">
            {{ item }}
        </Item>
    </div>
</template>

<script>
    import Item from "@/components/Item.vue"

    export default {
        name: "Board",
        components: {
            Item
        },
        props: [
            "items",
            "boardIndex",
            "newItem",
            "newItemBoardIndex",
            "draggableItemIndex",
            "sourceBoardIndex",
            "target",
            "properties",
            "itemHeight"
        ],
    }
</script>

<style scoped>
    .board {
        background-color: white;
        display: flex;
        flex-direction: column;
    }
</style>