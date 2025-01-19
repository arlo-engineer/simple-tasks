<script setup lang="ts">
const props = defineProps<{
    filterStatus: number;
    sortKey: string;
}>();

const emit = defineEmits<{
    "update:filterStatus": (value: number) => void;
    "update:sortKey": (value: string) => void;
}>();

function handleFilterChange(event: Event) {
    const target = event.target as HTMLSelectElement;
    emit("update:filterStatus", parseInt(target.value));
}

function handleSortChange(event: Event) {
    const target = event.target as HTMLSelectElement;
    emit("update:sortKey", target.value);
}
</script>

<template>
    <label for="filter">フィルター: </label>
    <select
        id="filter"
        :value="props.filterStatus"
        @change="handleFilterChange"
    >
        <option value="0">全て</option>
        <option value="1">未完了</option>
        <option value="2">完了済み</option></select
    ><br />

    <label for="sort">並び替え: </label>
    <select id="sort" :value="props.sortKey" @change="handleSortChange">
        <option value="due_date">締切日順</option>
        <option value="created_at">作成日順</option>
    </select>
</template>
