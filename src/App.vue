<template>
  <div>
    <Header/>
    <div class="container">
      <Balance :balance="balance" />
      <IncomeExpenses :income="+income" :expenses="+expenses" />
      <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted"/>
      <AddTransaction @transactionSubmitted="handleTransactionSubmitted"/>
    </div>
  </div>
</template>

<script setup>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpenses from './components/IncomeExpenses.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';

import { ref, computed,onMounted} from 'vue';
import {useToast} from 'vue-toastification'


const transactions = ref([])

      onMounted(()=> {
        const storedTransactions = JSON.parse(localStorage.getItem('transactions'))
        if(storedTransactions){
          transactions.value = storedTransactions._value
        }
      })
      const toast = useToast()
      
      // Get Income

      const income = computed(()=>{
        return transactions.value.filter((transaction)=> transaction.amount > 0)
        .reduce((acc, transaction)=>{
          return acc + transaction.amount
        },0)
      })

      // Get Expenses
      const expenses = computed(()=>{
        return transactions.value.filter((transaction)=> transaction.amount < 0)
        .reduce((acc, transaction)=>{
          return acc + transaction.amount
        },0)
      })

      // Get Balance
      const balance = computed(()=>{
          return income.value + expenses.value
        })

      // Add Transaction
      const handleTransactionSubmitted=(transaction)=>{
        const newTransaction = {
          id:generateUniqueId(),
          text:transaction.text,
          amount:transaction.amount,
        }
        transactions.value = [...transactions.value,newTransaction];
        storeTransactionsToLocalStorage();
        toast.success('Transaction added successfully')
      }

      // Delete transaction

      function handleTransactionDeleted(id){
        transactions.value = transactions.value.filter((eachTran) => eachTran.id !== id)
        storeTransactionsToLocalStorage()
        toast.success('Transaction deleted successfully')
      }

      // store transactions in localStorage
      const storeTransactionsToLocalStorage = () => {
        localStorage.setItem('transactions',JSON.stringify(transactions))
      }

      const generateUniqueId = () => {
        return Math.floor(Math.random() * 10000000)
      }


  
</script>

<style lang="scss" scoped>

</style>