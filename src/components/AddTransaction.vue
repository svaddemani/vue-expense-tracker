<script setup>
import { ref } from 'vue';
const props = defineProps(["TRANSACTION_TYPES"])
const title = ref("")
const amount = ref("")
const typeSelected = ref(props.TRANSACTION_TYPES.INCOME)
const emit = defineEmits(["addTransaction"])

const handleSubmit = () => {
  emit('addTransaction', { title: title.value, amount: amount.value, type: typeSelected.value });
  title.value = "",
    amount.value = "",
    typeSelected.value = props.TRANSACTION_TYPES.INCOME
}

</script>


<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="handleSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input type="text" id="text" v-model="title" placeholder="Enter text..." />
    </div>
    <div class="form-control">
      <label for="amount">Amount </label>
      <input type="radio" id="Input" :value="TRANSACTION_TYPES.INCOME" v-model="typeSelected" /> <label
        for="Income">Income</label>
      <input type="radio" id="Expense" :value="TRANSACTION_TYPES.EXPENSE" v-model="typeSelected" /> <label
        for="Expense">Expense</label>
      <input type="number" id="amount" v-model="amount" placeholder="Enter amount..." />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>