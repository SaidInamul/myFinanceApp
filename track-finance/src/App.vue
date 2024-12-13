<script setup>
  import Header from './components/header.vue'
  import Balance from './components/balance.vue'
  import incomeExpenses from './components/income-expenses.vue'
  import transactionList from './components/transaction-list.vue'
  import addTransaction from './components/add-transaction.vue'
  import { ref, computed, onMounted } from 'vue'
  import { useToast } from 'vue-toastification'


  const transactions = ref([])
  const toast = useToast()
  
  onMounted(() => {
    const savedTransactions = JSON.parse(localStorage.getItem('transactions'))
    if (savedTransactions) {
      transactions.value = savedTransactions
    }
  })

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
    saveIntoLocalStorage(0)
    toast.success('New transaction has been added')
  }

  // Delete transaction
  const handleDeleteTransaction = (id) => {
    transactions.value = transactions.value.filter((transaction) => transaction.id !== id)
    saveIntoLocalStorage()
    toast.success('The transaction is deleted')
  }

  // edit transaction
  const handleEditTransaction = (updatedTransaction) => {
    const index = transactions.value.findIndex(
      (transaction) => transaction.id === updatedTransaction.id
    );
    if (index !== -1) {
      transactions.value[index] = updatedTransaction;
    }
    saveIntoLocalStorage()
    toast.success('The transaction is updated')

  }

  // Save to local storage
  const saveIntoLocalStorage = () => {
    localStorage.setItem('transactions', JSON.stringify(transactions.value))
  }
</script>

<template>
    <Header />
    <div class="container">
      <Balance :total="+total"/>
      <incomeExpenses :income="income" :expenses="expenses"/>
      <transactionList :transactions="transactions" @deleteTransaction="handleDeleteTransaction" @editTransaction="handleEditTransaction"/>
      <addTransaction @transactionSubmitted="handleTransactionSubmitted"/>
    </div>
    
</template>