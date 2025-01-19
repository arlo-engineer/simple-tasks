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

enum FilterStatus {
    All = 0,
    Incomplete = 1,
    Complete = 2,
}

enum SortStatus {
    DueDate = "due_date",
    CreatedAt = "created_at",
}

const filterCompletedStatus = ref<FilterStatus>(FilterStatus.All);
const sortStatus = ref<SortStatus>(SortStatus.DueDate);

// フィルター後のタスクリスト
const filteredAndSortedTasks = computed(() => {
    let tasks = props.tasks;
    // フィルタリング
    if (filterCompletedStatus.value === FilterStatus.Incomplete) {
        tasks = tasks.filter((task) => task.completed === 0);
    } else if (filterCompletedStatus.value === FilterStatus.Complete) {
        tasks = tasks.filter((task) => task.completed === 1);
    }

    // ソート処理
    if (sortStatus.value === SortStatus.DueDate) {
        tasks = tasks.slice().sort((a, b) => {
            return (
                new Date(a.due_date).getTime() - new Date(b.due_date).getTime()
            );
        });
    } else if (sortStatus.value === SortStatus.CreatedAt) {
        tasks = tasks.slice().sort((a, b) => {
            return (
                new Date(a.created_at).getTime() -
                new Date(b.created_at).getTime()
            );
        });
    }

    return tasks;
});
</script>
<template>
    <h2>一覧ページ</h2>
    <Link :href="route('tasks.create')">新規追加</Link><br />
    <label for="filter">フィルター: </label>
    <select id="filter" v-model="filterCompletedStatus">
        <option :value="FilterStatus.All">全て</option>
        <option :value="FilterStatus.Incomplete">未完了</option>
        <option :value="FilterStatus.Complete">完了済み</option></select
    ><br />

    <label for="sort">並び替え: </label>
    <select id="sort" v-model="sortStatus">
        <option :value="SortStatus.DueDate">締切日順</option>
        <option :value="SortStatus.CreatedAt">作成日順</option>
    </select>
    <TaskListTable :tasks="filteredAndSortedTasks" />
</template>
