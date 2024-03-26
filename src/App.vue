<template>
    <main>
        <List/>
    </main>
</template>

<script>
    import List from "@/components/List.vue"

    export default {
        name: "App",
        components: {
            List
        }
    }
</script>

<style scoped>
    main {
        display: flex;
        align-items: center;
        min-height: 100vh;
    }
</style>