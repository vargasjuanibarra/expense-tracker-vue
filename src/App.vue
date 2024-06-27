<template>
    <Header />
    <div class="container">
      <Balance :total="total"/>
      <IncomeExpense :income="+income" :expense="-expense"/>
      <TransactionList :transactions="transactions" @deleteTransactionId="deleteTransactionItem"/>
      <AddTransaction @submitTransaction="handleTransactionData"/>
    </div>
</template>



<script setup>
import { ref, computed, onMounted } from 'vue';
  import Header from './components/Header.vue';
  import Balance from './components/Balance.vue';
  import IncomeExpense from './components/IncomeExpense.vue';
  import TransactionList from './components/TransactionList.vue';
  import AddTransaction from './components/AddTransaction.vue';
  import { useToast } from 'vue-toastification';

  const toast = useToast();

  const transactions = ref([]);

  onMounted(() => {
    const getTransactions = JSON.parse(localStorage.getItem('transactions'));

    if(getTransactions) {
      transactions.value = getTransactions;
    }
  })

  const total = computed(() => {
    return transactions.value.reduce((acc, transaction) => {
      return acc + transaction.amount
    }, 0)
  })

  const income = computed(() => {
    return transactions.value
    .filter(transaction => transaction.amount > 0)
    .reduce((acc, income) => {
      return acc + income.amount
    },0).toFixed(2);
  });

  const expense = computed(() => {
    return transactions.value
    .filter(transaction => transaction.amount < 0)
    .reduce((acc, expense) => {
      return acc + expense.amount
    },0).toFixed(2);
  });

  const handleTransactionData = (transactionData) => {
    transactions.value.push({
      id: generateUniqueId(),
      text: transactionData.text,
      amount: transactionData.amount
    })
    saveToLocalStorage();

    toast.success('Transaction added successfully', { timeout: 2000});
    console.log(transactionData);
  }

  const deleteTransactionItem = (transactionId) => {
    transactions.value = transactions.value.filter(item => item.id !== transactionId)
    saveToLocalStorage();
  }

  const generateUniqueId = () => {
    return Math.floor(Math.random() * 1230986)
  }

  const saveToLocalStorage = () => {
    const saveTransactionsToLocalStorage = localStorage.setItem('transactions', JSON.stringify(transactions.value))
  }

</script>
