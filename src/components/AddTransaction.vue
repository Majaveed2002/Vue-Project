<template>
  <div>
    <h3>Add new transaction</h3>
    <form id="form" @submit.prevent="addTransaction">
      <div class="form-control">
      <label for="text">Text</label>
      <input type="text" id="text" placeholder="Enter text..." v-model="text">
    </div>
    <div class="form-control">
      <label for="amount">Amount <br/>
      (negative - expense, positive - income)</label>
      <input type="text" id="amount" placeholder="Enter amount..." v-model="amount">
    </div>
    <button type="submit" class="btn">Add transaction</button>
    </form>
  </div>
</template>

<script setup>
import {  ref, } from 'vue'
import {useToast} from 'vue-toastification'
  

const text = ref('')
const amount = ref('')

const emit = defineEmits(['transactionSubmitted'])

const toast = useToast()

      function addTransaction(){
        if(!text.value || !amount.value){
            toast.error('Both fiedls must be filled')
            return;
        }
        const newTransaction = {
          text:text.value,
          amount:parseFloat(amount.value),
        }

        emit('transactionSubmitted',newTransaction)

        text.value = ''
        amount.value = ''
      } 
</script>

<style scoped>

</style>