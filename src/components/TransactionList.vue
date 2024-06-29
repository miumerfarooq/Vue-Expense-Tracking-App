<template>
    <div>
        <h2 class="is-size-5 py-1 my-2 bb">History</h2>
        <ul>
            <li v-if="transactions.length > 0" v-for="transaction in transactions" :key="transaction.id"
                class="has-background-primary-40 mb-2 p-2 is-flex is-justify-content-space-between"
                :class="[transaction.amount >= 0 ? 'plus': 'minus']">
                <span><button class="close__button" @click="deleteTransaction(transaction.id)">X</button>{{ transaction.text }}</span>
                <span>{{ transaction.amount >= 0 ? `$${transaction.amount}` : `-$${Math.abs(transaction.amount)}` }}</span>
            </li>
            <li v-else class="has-background-primary-40 mb-2 p-2 is-flex is-justify-content-center">No Transaction Found</li>
        </ul>
    </div>
</template>

<script setup>
// import {toRefs} from "vue";
// const props = defineProps(['transactions'])
defineProps(['transactions'])
// const { transactions } = toRefs(props);
const emit = defineEmits(['transactionDeleted'])

const deleteTransaction = (transactionId) => {
    emit('transactionDeleted', transactionId)
}
</script>

<style>
/* li {
    position: relative;
}
li::before {
    content: 'X';
    position: absolute;
    left: 0px;
    width: 30px;
    background-color: red;
    color: white;
    text-align: center;
} */
.close__button {
    padding: 0px 15px;
    background-color: red;
    color: white;
    display: inline-block;
    text-align: center;
    position: relative;
    left: -8px;
    display: none;
    opacity: 0;
    transition: opacity 0.3s ease-in-out;
}
li:hover .close__button {
    display: inline-block;
    opacity: 1;
}
</style>
