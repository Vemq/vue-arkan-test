<script setup lang="ts">
defineProps<{
    selected?: boolean;
    done?: boolean;
}>()
</script>

<template>
    <div class="list-item" :class="{selected, done}">
        <slot></slot>
    </div>
</template>

<style scoped>
.list-item {
    padding: 2px 5px;
    border: 1px solid gainsboro;
    border-radius: 2px;
    user-select: none;
    cursor: pointer;
}

.done {
    cursor: inherit;
    background-color: GhostWhite;
}

.selected {
    border: 1px solid green;
    background-color: Honeydew;
}
</style>