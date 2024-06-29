<template>
    <div class="mb-4">
        <h2 class="is-size-5">Balance</h2>
        <p class="is-size-5 has-text-weight-bold">{{ totalBalance >= 0 ? `$${totalBalance}` : `-$${Math.abs(totalBalance)}` }}</p>
    </div>
</template>

<script setup>
const props = defineProps({
    totalBalance: {
        type: Number,
        default: 0
    }
})
</script>

<style></style>
