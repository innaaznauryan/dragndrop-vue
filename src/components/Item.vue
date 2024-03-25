<template>
    <p draggable="true" class="item">
        <slot/>
    </p>
</template>

<script>
    export default {
        name: "Item",
    }
</script>

<style scoped>
    .item {
        padding: 10px;
        box-shadow: inset 0 0 7px gray;
        transition: .5s;
    }

    .new {
        color: darkgreen;
    }

    .transform {
        transform: translateY(38px);
    }

    .transparent {
        opacity: 0;
    }
</style>