<template>
    <p
            draggable="true"
            class="item"
            :style="{'height': `${itemHeight}px`}">
        <slot/>
    </p>
</template>

<script>
    export default {
        name: "Item",
        props: ["itemHeight"]
    }
</script>

<style scoped>
    .item {
        padding: 10px;
        transition: .5s;
    }

    .new {
        color: seagreen;
        text-shadow: 0 0 5px darkseagreen;
    }

    .transform {
        transform: translateY(40px);
    }

    .transparent {
        opacity: 0;
    }
</style>