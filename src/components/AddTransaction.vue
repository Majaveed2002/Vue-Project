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
    <button class="btn">Add transaction</button>
    </form>
  </div>
</template>

<script>
import { reactive, toRefs, } from 'vue'
import {useToast} from 'vue-toastification'
  export default {
    name:'AddTransaction',
    props:['transactions'],
    setup(props,context){
      const formData = reactive({
        text:'',
        amount:''
      })


      const toast = useToast()

      function addTransaction(){
        if(!formData.text || !formData.amount){
            toast.error('Both fiedls must be filled')
            return;
        }
        const length = props.transactions.length - 1
        const newTransaction = {
          id:props.transactions[length].id+1,
          text:formData.text,
          amount:parseFloat(formData.amount),
        }
        context.emit('transactionSubmitted',newTransaction)

        formData.text = ""
        formData.amount = ''
      }

      return{
        ...toRefs(formData),
        addTransaction,
      }
    }
  }
</script>

<style scoped>

</style>