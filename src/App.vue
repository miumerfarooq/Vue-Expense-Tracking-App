<script setup>
import { ref, computed, onMounted } from 'vue'
import { useToast } from "vue-toastification"
import Header from '@/components/Header.vue'
import Balance from '@/components/Balance.vue'
import IncomeAndExpense from '@/components/IncomeAndExpense.vue'
import TransactionList from '@/components/TransactionList.vue'
import AddTransaction from '@/components/AddTransaction.vue'

const toast = useToast()

const transactions = ref([])

// onMounted(() => {
//   const savedTransactions = JSON.parse(localStorage.getItem('transactions'))
//   transactions.value = savedTransactions || []

//   // if (savedTransactions) {
//   //   transactions.value = savedTransactions
//   // }
// })

onMounted(() => {
  try {
    const savedTransactions = JSON.parse(localStorage.getItem('transactions'));
    transactions.value = Array.isArray(savedTransactions) ? savedTransactions : [];
  } catch (error) {
    toast.error(error.message)
    //transactions.value = [];
  }
})

// Total Balance
const totalBalance = computed(() => {
  return parseFloat(transactions.value.reduce((accu, transaction) => accu + transaction.amount, 0))
})

// Total Income
const income = computed(() => {
  return parseFloat(transactions.value.filter((transaction) => transaction.amount > 0)
  .reduce((accu, transaction) => accu + transaction.amount, 0).toFixed(2))
})

// Total Expense
const expense = computed(() => {
  return parseFloat(transactions.value.filter((transaction) => transaction.amount < 0)
  .reduce((accu, transaction) => accu + transaction.amount, 0).toFixed(2))
})

// Add new Transaction
const handleTransactionSubmitted = (transactionData) => {
  const maxId = transactions.value.length > 0 ? Math.max(...transactions.value.map(t => t.id)) : 0
  const newTransaction = {
    id: maxId + 1,
    text: transactionData.title,
    amount: transactionData.amount
  }
  transactions.value.push(newTransaction)
  saveTransactionsToLocalStorage()
  toast.success('Transaction Added Successfully')
}

// handle Delete Transaction
const hanldeTransactionDeleted = (transactionId) => {
  const index = transactions.value.findIndex((transaction) => transaction.id === transactionId);
  if (index !== -1) {
    transactions.value.splice(index, 1);
  }
  saveTransactionsToLocalStorage()
  toast.success('Transaction Deleted Successfully')
  // console.log(transactionId)
}

// Localstorage
const saveTransactionsToLocalStorage = () => {
  localStorage.setItem('transactions', JSON.stringify(transactions.value))
}
</script>

<template>
    <div class="hero is-small is-primary p-5">
      <p class="title">Expense Tracking App</p>
    </div>
    <div class="columns is-centered mt-0 py-4">
      <div class="column is-half has-background-primary-35 card">
          <Header />
          <Balance :totalBalance="totalBalance" />
          <IncomeAndExpense :income="income" :expense="expense" />
          <TransactionList :transactions="transactions" @transactionDeleted="hanldeTransactionDeleted" />
          <AddTransaction @transactionSubmitted="handleTransactionSubmitted" />
      </div>
    </div>
</template>

<style>
@import 'bulma/css/bulma.min.css';
@import url('https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap');
body {
  font-family: 'Poppins', sans-serif;
}
</style>
