<script setup lang="ts">
import { Link } from "@inertiajs/vue3";
import { computed, ref } from "vue";
import TaskListTable from "@/Components/TaskListTable.vue";

interface Task {
    id: number;
    name: string;
    notes?: string;
    completed: number;
    due_date: string;
    created_at: string;
}

const props = defineProps<{
    tasks: Task[];
}>();

const filterCompletedStatus = ref(0);
const filterCompletedTasks = computed(() => {
    if (filterCompletedStatus.value === 0) {
        return props.tasks;
    } else if (filterCompletedStatus.value === 1) {
        return props.tasks.filter((task) => task.completed === 0);
    } else if (filterCompletedStatus.value === 2) {
        return props.tasks.filter((task) => task.completed === 1);
    }
});

function handleChangeFilter(event: Event) {
    const target = event.target as HTMLSelectElement;
    filterCompletedStatus.value = parseInt(target.value);
}
</script>
<template>
    <h2>一覧ページ</h2>
    <Link :href="route('tasks.create')">新規追加</Link>
    <select @change="handleChangeFilter">
        <option value="0">全て</option>
        <option value="1">未完了</option>
        <option value="2">完了済み</option>
    </select>
    <TaskListTable :tasks="filterCompletedTasks" />
</template>
