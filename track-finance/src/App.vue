<script setup>
  import Header from './components/header.vue'
  import Balance from './components/balance.vue'
  import incomeExpenses from './components/income-expenses.vue'
  import transactionList from './components/transaction-list.vue'
  import addTransaction from './components/add-transaction.vue'
  import { ref, computed } from 'vue'

  const transactions = ref([
    {id : 1, name : 'Rental', price : -800},
    {id : 2, name : 'Food', price : -1000},
    {id : 3, name : 'PayCheck', price : 3000},
    {id : 4, name : 'Telco', price : -50},
  ])

  const total = computed( () => {
    return transactions.value.reduce((acc, transaction) => {
      return acc + transaction.price
    }, 0)
  })
</script>

<template>
    <Header />
    <div class="container">
      <Balance :total="total"/>
      <incomeExpenses />
      <transactionList :transactions="transactions"/>
      <addTransaction />
    </div>
    
</template>