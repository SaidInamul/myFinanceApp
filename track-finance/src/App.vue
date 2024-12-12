<script>
  import Header from './components/header.vue'
  import Balance from './components/balance.vue'
  import incomeExpenses from './components/income-expenses.vue'
  import transactionList from './components/transaction-list.vue'
  import addTransaction from './components/add-transaction.vue'

  export default {
    components : {
      Header, Balance, incomeExpenses, transactionList, addTransaction
    }
  }

</script>

<template>
    <Header />
    <div class="container">
      <Balance />
      <incomeExpenses />
      <transactionList />
      <addTransaction />
    </div>
    
</template>