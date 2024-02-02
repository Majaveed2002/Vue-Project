<template>
  <div>
    <h3>History</h3>
    <ul id="list" class="list">
      <li class="minus">Cash <span>-$400</span>
      <button class="delete-btn">X</button>
      </li>
      <li class="plus">Paycheck <span>+$800</span>
      <button class="delete-btn">X</button>
      </li>
      
    </ul>
  </div>
</template>

<script>
  export default {
    name:'TransactionList'
  }
</script>

<style scoped>

</style>