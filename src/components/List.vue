<template>
    <div class="container">
        <div class="list">
            <div
                    v-for="(board, index) in boards"
                    :key="'board' + index"
                    class="board-wrapper">
                <h3>{{ properties[index].title }}</h3>
                <Board
                        :items="board"
                        :boardIndex="index"
                        :newItemIndex="newItemIndex"
                        :newItemBoardIndex="newItemBoardIndex"
                        :draggableItemIndex="draggableItemIndex"
                        :sourceBoardIndex="sourceBoardIndex"
                        :target="target"
                        :properties="properties"
                        @dragstart="e => handleDragStart(e, index)"
                        @dragover="e => handleDragOver(e, index)"
                        @drop="e => handleDrop(e, index)"
                        @dragleave="e => handleDragLeave(e)"
                        @dragend="handleDragend"/>
            </div>
        </div>
        <form @submit.prevent="handleSubmit">
            <input type="text" placeholder="New task" required v-model.trim="newItemTitle">
            <select v-model="newItemBoardIndex">
                <option
                        v-for="(board, index) in boards"
                        :key="'option' + index"
                        :value="index">
                    {{ properties[index].title }}
                </option>
            </select>
            <button type="submit">Add</button>
        </form>
    </div>
</template>

<script>
    import Board from "@/components/Board.vue"
    import dropSound from "@/assets/audio/dropSound.mp3"

    export default {
        name: 'List',
        components: {
            Board
        },
        data() {
            return {
                newItemTitle: null,
                newItemIndex: null,
                newItemBoardIndex: 0,
                draggableItemIndex: null,
                sourceBoardIndex: null,
                target: {itemIndex: null, boardIndex: null},
                boards: [
                    ["burunduk1", "burunduk2", "burunduk3"],
                    ["burunduk4", "burunduk5", "burunduk6"],
                    ["burunduk7", "burunduk8", "burunduk9"],
                ],
                properties: [
                    {title: "To do", color: "lightcyan"},
                    {title: "In progress", color: "lightyellow"},
                    {title: "Completed", color: "lightgreen"}
                ]
            }
        },
        methods: {
            handleSubmit() {
                this.boards[this.newItemBoardIndex].push(this.newItemTitle)
                this.newItemIndex = this.boards[this.newItemBoardIndex].length - 1
                this.newItemTitle = null
            },
            handleDragStart(e, boardIndex) {
                if (e.target.classList.contains("item")) {
                    this.draggableItemIndex = +e.target.dataset.index
                    this.sourceBoardIndex = boardIndex
                    e.dataTransfer.effectAllowed = "move"
                }
            },
            handleDragOver(e, boardIndex) {
                e.preventDefault()
                this.target.boardIndex = boardIndex
                if (boardIndex === this.sourceBoardIndex) {
                    e.dataTransfer.dropEffect = "none"
                    return
                }
                if (e.target.classList.contains("item")) {
                    if (e.clientY > e.target.getBoundingClientRect().y + e.target.getBoundingClientRect().height / 2) {
                        this.target.itemIndex = +e.target.dataset.index + 1
                    } else {
                        this.target.itemIndex = +e.target.dataset.index
                    }
                }
                if (e.target.classList.contains("board")) {
                    if (e.clientY > e.target.getBoundingClientRect().y + this.boards[boardIndex].length * 38) {
                        this.target.itemIndex = this.boards[boardIndex].length
                    }
                }
            },
            handleDrop(e, boardIndex) {
                const draggableItem = this.boards[this.sourceBoardIndex][this.draggableItemIndex]
                this.boards[boardIndex].splice(this.target.itemIndex, 0, draggableItem)
                this.boards[this.sourceBoardIndex].splice(this.draggableItemIndex, 1)
                this.draggableItemIndex = null
                this.sourceBoardIndex = null
                this.target.itemIndex = null
                this.target.boardIndex = null
                const sound = new Audio(dropSound)
                sound.play()
            },
            handleDragLeave(e) {
                if (e.target.classList.contains("board")) {
                    this.target.itemIndex = null
                    this.target.boardIndex = null
                }
            },
            handleDragend(e) {
                console.log("dragend")
                this.draggableItemIndex = null
                this.sourceBoardIndex = null
                this.target.itemIndex = null
                this.target.boardIndex = null
            }
        },
    }
</script>

<style scoped>
    .container {
        margin: 20px auto;
        max-width: 1000px;
    }

    .list {
        display: flex;
        justify-content: center;
        gap: 10px;
        min-height: 300px;
    }

    .board-wrapper {
        flex: 1;
        aspect-ratio: 1;
    }

    h3 {
        text-align: center;
        padding: 10px;
    }

    form {
        margin: 20px auto;
        max-width: 50%;
        display: flex;
        justify-content: center;
        gap: 10px;
    }

    input {
        padding: 15px;
        background-color: lightgray;
        border: 0;
    }

    select {
        padding: 15px;
        background-color: lightgray;
        border: 0;
    }

    button {
        padding: 10px 20px;
        cursor: pointer;
        background-color: black;
        color: white;
        border: 0;
    }
</style>