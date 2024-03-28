<script setup>
import Header from './components/Header.vue';
import Balence from './components/Balence.vue';
import IncomeExpenses from "./components/IncomeExpenses.vue";
import History from "./components/History.vue"
import AddTransaction from "./components/AddTransaction.vue"
import { computed, ref, onMounted } from 'vue';
import { useToast } from "vue-toastification";

const TRANSACTION_TYPES = {
  EXPENSE: "EXPENSE",
  INCOME: "INCOME"
}

const transactions = ref([])

const income = computed(() => transactions.value.filter(item => item.type === TRANSACTION_TYPES.INCOME).reduce((acc, item) => acc + item.amount, 0))
const expense = computed(() => transactions.value.filter(item => item.type === TRANSACTION_TYPES.EXPENSE).reduce((acc, item) => acc + item.amount, 0))
const amountRemaining = computed(() => income.value - expense.value) || 0
const toast = useToast()

onMounted(() => {
  // added settimeut to fix toast not visible after refresh issue
  setTimeout(() => {
    toast.info("This uses local storage & so data can be seen only in this device")
  }, 0);
})

onMounted(() => {
  const savedTransactions = JSON.parse(localStorage.getItem("transactions"));
  if (savedTransactions) {
    transactions.value = savedTransactions
  }
})

const addTransaction = (transactiondata) => {
  const newTransaction = {
    id: transactions.value.length + 1,
    title: transactiondata.title,
    amount: transactiondata.amount,
    type: transactiondata.type
  }
  transactions.value.push(newTransaction);
  localStorage.setItem("transactions", JSON.stringify(transactions.value))
  toast.success('Successfully added record')
}

const removeTransaction = (transactiondata) => {
  transactions.value = transactions.value.filter(item => item.id !== transactiondata.id)
  localStorage.setItem("transactions", JSON.stringify(transactions.value))
  toast.success('Successfully removed record')
}

</script>

<template>
  <Header />
  <Balence :balence="amountRemaining" />
  <IncomeExpenses :expense="expense" :income="income" />
  <History :transactions="transactions" @removeTransaction="removeTransaction" :TRANSACTION_TYPES="TRANSACTION_TYPES" />
  <AddTransaction @addTransaction="addTransaction" :TRANSACTION_TYPES="TRANSACTION_TYPES" />
</template>
