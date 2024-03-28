<script setup>
import { ref } from 'vue';

const props = defineProps(["transactions", "TRANSACTION_TYPES"])
const emit = defineEmits(["removeTransaction"])


const handleRemove = (item) => {
  emit('removeTransaction', { id: item.id })
}

</script>


<template>
  <h3>History</h3>
  <p v-if="!transactions.length"> Add a transaction to start...</p>
  <ul id="list" class="list">
    <li v-for="transaction in transactions" :key="transaction.id"
      :class="transaction.type === TRANSACTION_TYPES.EXPENSE ? 'minus' : 'plus'">
      {{ transaction.title }} <span> {{ transaction.type === TRANSACTION_TYPES.EXPENSE ? '-' : '+' }} {{
    "$" + transaction.amount }}</span><button class="delete-btn" @click="handleRemove(transaction)">x</button>
    </li>
  </ul>
</template>