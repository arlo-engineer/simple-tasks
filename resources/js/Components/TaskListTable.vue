<script setup lang="ts">
import { Link, router } from "@inertiajs/vue3";

const props = defineProps<{
    tasks: Array<{
        id: number;
        name: string;
        completed: number;
        due_date: string;
        created_at: string;
    }>;
}>();

function toggleCompleted(taskId: number) {
    return router.put(`/tasks/${taskId}/toggle-completed`);
}
</script>
<template>
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
            <tr v-for="task in props.tasks" :key="task.id">
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
