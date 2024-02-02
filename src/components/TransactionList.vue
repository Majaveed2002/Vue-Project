<template>
  <div>
    <h3>History</h3>
    <ul id="list" class="list">
      <li v-for="transaction in transactions" :key="transaction.id" :class="transaction.amount < 0 ? 'minus':'plus'">{{ transaction.text }}
        <span>{{ transaction.amount }}</span>
        <button class="delete-btn">X</button>
      </li>
    </ul>
  </div>
</template>

<script>

  export default {
    name:'TransactionList',
    props:['transactions']
    
  }
</script>

<style scoped>

</style>