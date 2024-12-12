<script setup>
  import Header from './components/header.vue'
  import Balance from './components/balance.vue'
  import incomeExpenses from './components/income-expenses.vue'
  import transactionList from './components/transaction-list.vue'
  import addTransaction from './components/add-transaction.vue'
  import { ref } from 'vue'

  const transactions = ref([
    {id : 1, name : 'Rental', price : -800},
    {id : 2, name : 'Food', price : -1000},
    {id : 3, name : 'PayCheck', price : 2000},
    {id : 4, name : 'Telco', price : -50},
  ])
</script>

<template>
    <Header />
    <div class="container">
      <Balance />
      <incomeExpenses />
      <transactionList :transactions="transactions"/>
      <addTransaction />
    </div>
    
</template>