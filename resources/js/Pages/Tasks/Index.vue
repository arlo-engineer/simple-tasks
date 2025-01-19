<script setup lang="ts">
import { Link } from "@inertiajs/vue3";
import { computed, ref } from "vue";
import TaskListTable from "@/Components/TaskListTable.vue";
import FilterControls from "@/Components/FilterControls.vue";

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

const filterStatus = ref(0); // 0: 全て, 1: 未完了, 2: 完了済み
const sortKey = ref("due_date"); // due_date: 締切日順, created_at: 作成日順

// フィルタリングとソート後のタスクリスト
const filteredAndSortedTasks = computed(() => {
    // フィルタリング処理
    const filteredTasks = props.tasks.filter((task) => {
        if (filterStatus.value === 1) return task.completed === 0;
        if (filterStatus.value === 2) return task.completed === 1;
        return true;
    });

    // ソート処理
    return filteredTasks.slice().sort((a, b) => {
        if (sortKey.value === "due_date") {
            return a.due_date.localeCompare(b.due_date);
        }
        return a.created_at.localeCompare(b.created_at);
    });
});
</script>

<template>
    <h2>一覧ページ</h2>
    <Link :href="route('tasks.create')">新規追加</Link><br />
    <FilterControls
        v-model:filterStatus="filterStatus"
        v-model:sortKey="sortKey"
    />
    <TaskListTable :tasks="filteredAndSortedTasks" />
</template>
