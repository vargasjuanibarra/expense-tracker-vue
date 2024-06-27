<template>
    <h3>
       Add Transaction 
    </h3>
    <form id="form" @submit.prevent="onSubmit()">
        <div class="form-control">
            <label for="text">Text</label>
            <input type="text" id="text" placeholder="Enter text" v-model="label">
        </div>

        <div class="form-control">
            <label for="amount">Amount <br/>
            (negative - expense, positive - income)</label>
            <input type="text" id="amout" placeholder="Enter amount..." v-model="amount">
        </div>
        <button class="btn">Add transaction</button>
    </form>
</template>

<script setup>
    import { ref } from 'vue';
    import { useToast } from 'vue-toastification';

    const label = ref('');
    const amount = ref();

    const toast = useToast();
    const emit = defineEmits(['submitTransaction'])

    const onSubmit = () => {

        if(!label.value || !amount.value) {
            toast.error('Please fill in all the fields', { timeout: 2000 });
            return;
        }

        const transactionValue = {
            text: label.value,
            amount: parseFloat(amount.value)
        }

        emit('submitTransaction', transactionValue)
        
        label.value = '';
        amount.value = 0;

    }
</script>