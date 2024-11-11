<script lang="ts" setup>
import { defineProps } from 'vue';
import ToDoItem from './ToDoItem.vue';
const props = defineProps({
    state: Boolean,
    items: Array<{
        id: number,
        title: string,
        details: string,
        status: boolean|undefined
    }>
});
    
console.log("list")
console.log(props.items)


</script>

<template>
    <v-card min-height="450px" width="20vw" class="ma-3 pa-5">
        <v-title v-if="state">Active</v-title>
        <v-title v-else>Completed</v-title>
        <ToDoItem v-for="item in props.items" :id="item.id" :title="item.title" :details="item.details" :status="item.status" @remove="$emit('remove-item', item.id)" @toggle-done="$emit('toggle-done', item.id)"/>
    </v-card>
</template>

<style scoped>
/* Your styles here */
</style>