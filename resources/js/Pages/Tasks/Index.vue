<script setup lang="ts">
import { Link, router } from "@inertiajs/vue3";
import { computed, ref } from "vue";

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

function toggleCompleted(taskId: number) {
    return router.put(`/tasks/${taskId}/toggle-completed`);
}

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
    <table>
        <thead>
            <tr>
                <th>タスク名</th>
                <th>状態</th>
                <th>期日</th>
                <th>作成日</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="task in filterCompletedTasks" :key="task.id">
                <td>
                    <Link :href="route('tasks.show', task.id)">{{
                        task.name
                    }}</Link>
                </td>
                <td>
                    <button @click="toggleCompleted(task.id)">
                        {{ task.completed ? "✅完了済み" : "⬜未完了" }}
                    </button>
                </td>
                <td>{{ task.due_date }}</td>
                <td>
                    {{ task.created_at }}
                </td>
            </tr>
        </tbody>
    </table>
</template>
