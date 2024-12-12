<script setup>
  import Header from './components/header.vue'
  import Balance from './components/balance.vue'
  import incomeExpenses from './components/income-expenses.vue'
  import transactionList from './components/transaction-list.vue'
  import addTransaction from './components/add-transaction.vue'
  import { ref, computed } from 'vue'
  import { useToast } from 'vue-toastification'


  const transactions = ref([
    {id : 1, name : 'Rental', price : -500},
    {id : 2, name : 'Food', price : -1000},
    {id : 3, name : 'PayCheck', price : 3000},
    {id : 4, name : 'Telco', price : -50},
  ])
  const toast = useToast()

  // Get total
  const total = computed( () => {
    return transactions.value.reduce((acc, transaction) => {
      return acc + transaction.price
    }, 0)
  })

  // Get income
  const income = computed( () => {
    return parseFloat(transactions.value
    .filter((transaction) =>
      transaction.price >= 0
    )
    .reduce((acc, transaction) => {
      return acc + transaction.price
    }, 0)
    .toFixed(2))
  })

  // Get expenses
  const expenses = computed( () => {
    return parseFloat(transactions.value
    .filter((transaction) => 
      transaction.price < 0
    )
    .reduce((acc, transaction) => {
      return acc + transaction.price
    }, 0)
    .toFixed(2))
  })

  // Add new transaction
  const handleTransactionSubmitted = (transactionData) => {
    transactions.value.push({
      id : crypto.randomUUID(),
      price : transactionData.price,
      name : transactionData.text
    })

    toast.success('New transaction has been added')
  }

  // Delete transaction
  const handleDeleteTransaction = (id) => {
    transactions.value = transactions.value.filter((transaction) => transaction.id !== id)

    toast.success('The transaction is deleted')
  }
</script>

<template>
    <Header />
    <div class="container">
      <Balance :total="+total"/>
      <incomeExpenses :income="income" :expenses="expenses"/>
      <transactionList :transactions="transactions" @deleteTransaction="handleDeleteTransaction"/>
      <addTransaction @transactionSubmitted="handleTransactionSubmitted"/>
    </div>
    
</template>