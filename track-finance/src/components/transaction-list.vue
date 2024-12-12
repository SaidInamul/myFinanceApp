<script setup>
    import { defineProps } from 'vue';

    const props = defineProps({
        transactions : {
            type : Array,
            required : true
        }
    })

    const emit = defineEmits([
        'deleteTransaction'
    ])
    
    const deleteTransaction = (id) => {
        emit('deleteTransaction', id)
    }


</script>

<template>
    <h3>History</h3>
      <ul id="list" class="list">
        <li v-for="transaction in transactions"
            key="transaction.id"
            :class="transaction.price < 0 ? 'minus' : 'plus'">
            {{ transaction.name }} 
            <span>${{ transaction.price }}</span>
            <button @click="deleteTransaction(transaction.id)" class="delete-btn">x</button>
        </li>
      </ul>
</template>