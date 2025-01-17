<script setup lang="ts">
import { ref } from "vue";
import { Link, router } from "@inertiajs/vue3";

const form = ref({
    name: "",
    notes: "",
    due_date: "",
});
function addTasks() {
    router.post("/tasks/create", form.value);
}
</script>
<template>
    <h2>新規登録ページ</h2>
    <form @submit.prevent="addTasks">
        <label for="name">タスク名</label><br />
        <input type="text" v-model="form.name" id="name" required /><br />
        <label for="notes">メモ</label><br />
        <textarea v-model="form.notes" id="notes"></textarea><br />
        <label for="due_date">期日</label><br />
        <input
            type="date"
            v-model="form.due_date"
            id="due_date"
            required
        /><br />
        <button @click="addTasks">追加</button><br />
        <Link :href="route('tasks.index')">戻る</Link>
    </form>
</template>
