<template>
    <div class="board">
        <Item
                v-for="(item, index) in items"
                :key="'item' + index"
                :data-index="index"
                :style="{'background-color': properties[boardIndex].color}"
                :class="{
                    'new': index === newItemIndex && boardIndex === newItemBoardIndex,
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
            "newItemIndex",
            "newItemBoardIndex",
            "draggableItemIndex",
            "sourceBoardIndex",
            "target",
            "properties"
        ],
    }
</script>

<style scoped>
    .board {
        background-color: white;
        display: flex;
        flex-direction: column;
        height: 100%;
    }
</style>