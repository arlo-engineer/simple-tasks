<script setup lang="ts">
import { Link } from "@inertiajs/vue3";
interface Task {
    id: number;
    name: string;
    notes?: string;
    due_date: string;
    created_at: string;
}

const props = defineProps<{
    task: Task;
}>();
</script>
<template>
    <h2>詳細ページ</h2>
    <p>タスク名: {{ task.name }}</p>
    <p>期日: {{ task.due_date }}</p>
    <p>作成日: {{ task.created_at }}</p>
    <p>メモ: {{ task.notes }}</p>
    <Link :href="route('tasks.index')">戻る</Link>
</template>
