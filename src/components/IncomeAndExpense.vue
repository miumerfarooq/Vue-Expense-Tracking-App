<template>
    <div class="columns has-background-primary-40">
        <div class="column br">
            <h2 class="is-size-5">Income</h2>
            <p class="is-size-5 has-text-weight-bold">${{ income }}</p>
        </div>
        <div class="column">
            <h2 class="is-size-5">Expense</h2>
            <p class="is-size-5 has-text-weight-bold">-${{ Math.abs(expense) }}</p>
        </div>
    </div>
</template>

<script setup>
defineProps({
    income: {
        type: Number,
        default: 0
    },
    expense: {
        type: Number,
        default: 0
    }
})
</script>

<style scoped>
.columns {
    margin-left: 0;
    margin-right: 0;
}
</style>
