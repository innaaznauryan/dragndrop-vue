<template>
    <div class="container">
        <div class="list">
            <div
                    v-for="(board, index) in boards"
                    :key="'board' + index"
                    class="board-wrapper">
                <h3 :style="{'height': `${itemHeight}px`}">
                    {{ properties[index].title }}
                </h3>
                <Board
                        :items="board"
                        :boardIndex="index"
                        :newItemId="newItemId"
                        :draggableItemIndex="draggableItemIndex"
                        :sourceBoardIndex="sourceBoardIndex"
                        :target="target"
                        :properties="properties"
                        :itemHeight="itemHeight"
                        @dragstart="e => handleDragStart(e, index)"
                        @dragover="e => handleDragOver(e, index)"
                        @drop="e => handleDrop(e, index)"
                        @dragleave="e => handleDragLeave(e)"
                        @dragend="handleDragend"/>
            </div>
        </div>
        <form
                @submit.prevent="handleSubmit"
                :style="{'margin': `${itemHeight}px auto 0`}">
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
                newItemId: null,
                newItemBoardIndex: 0,
                draggableItemIndex: null,
                sourceBoardIndex: null,
                target: {itemIndex: null, boardIndex: null},
                itemHeight: 40,
                boards: [
                    [{title: "Task 1", id: 1}, {title: "Task 2", id: 2}, {title: "Task 3", id: 3}],
                    [{title: "Task 4", id: 4}, {title: "Task 5", id: 5}, {title: "Task 6", id: 6}],
                    [{title: "Task 7", id: 7}, {title: "Task 8", id: 8}, {title: "Task 9", id: 9}],
                ],
                properties: [
                    {title: "To do", color: "cadetblue"},
                    {title: "In progress", color: "orange"},
                    {title: "Completed", color: "green"}
                ]
            }
        },
        methods: {
            handleSubmit() {
                const id = Math.round(Math.random() * 1000)
                const newItem = {id, title: this.newItemTitle}
                this.boards[this.newItemBoardIndex].push(newItem)
                this.newItemId = id
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
                    if (e.clientY > e.target.getBoundingClientRect().y + this.boards[boardIndex].length * this.itemHeight) {
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
        margin: 10px auto;
        max-width: 1000px;
        width: 100%;
    }

    .list {
        display: flex;
        justify-content: center;
        gap: 10px;
    }

    .board-wrapper {
        flex: 1;
    }

    h3 {
        text-align: center;
        padding: 10px;
    }

    form {
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
        background-color: lightgray;
        border: 0;
    }
</style>