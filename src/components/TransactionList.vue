<template>
  <h3>History</h3>
  <ul id="list" class="list">
    <li
      v-for="transaction in transactions"
      :key="transaction.id"
      :class="transaction.amount > 0 ? 'plus' : 'minus'"
    >
      {{ transaction.text }} <span>${{ transaction.amount }}</span>
      <button class="delete-btn" @click="deleteTransactionItem(transaction.id)">X</button>
    </li>
    <!-- <li class="plus">
            Salary <span>$1,200</span>
            <button class="delete-btn">X</button>
        </li> -->
  </ul>
</template>

<script setup>

    const emit = defineEmits(['deleteTransactionId'])

    const props = defineProps({
        transactions: {
            type: Array,
            required: true,
        }
    })

    const deleteTransactionItem = (id) => {
        emit('deleteTransactionId', id)
    }

</script>
