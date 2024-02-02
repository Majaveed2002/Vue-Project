<template>
  <div>
    <Header/>
    <div class="container">
      <Balance :balance="+balance" />
      <IncomeExpenses :income="+income" :expenses="+expenses" />
      <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted"/>
      <AddTransaction :transactions="transactions" @transactionSubmitted="handleTransactionSubmitted"/>
    </div>
  </div>
</template>

<script>
import Header from './components/Header.vue';
import Balance from './components/Balance.vue';
import IncomeExpenses from './components/IncomeExpenses.vue';
import TransactionList from './components/TransactionList.vue';
import AddTransaction from './components/AddTransaction.vue';

import { ref, computed } from 'vue';
import {useToast} from 'vue-toastification'
  export default {
    name:'App',
    components:{
    Header,
    Balance,
    IncomeExpenses,
    TransactionList,
    AddTransaction,
  },
  setup(){
      const transactions = ref([
        {id:1,text:'Flower',amount:-19.99},
        {id:2,text:'Salary',amount:229.97},
        {id:3,text:'Book',amount:-10},
        {id:4,text:'Camera',amount:150},
      ])
      const toast = useToast()
      // Get Balance
      const balance = computed(()=>{
        return transactions.value.reduce((acc,trans)=>{
          return acc + trans.amount
        },0)
      })

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

      // Add Transaction
      function handleTransactionSubmitted(data){
        transactions.value.push(data)
        toast.success('Transaction added successfully')
        return
      }

      // Delete transaction

      function handleTransactionDeleted(id){
        transactions.value = transactions.value.filter((eachTran) => eachTran.id !== id)
        toast.success('Transaction deleted successfully')
      }

      
      return {
        transactions,
        balance,
        income,
        expenses,
        handleTransactionSubmitted,
        handleTransactionDeleted,
      }
    }
  }
</script>

<style lang="scss" scoped>

</style>