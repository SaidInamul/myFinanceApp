<script setup>
    import { ref, defineProps } from 'vue';

    const props = defineProps({
        transactions : {
            type : Array,
            required : true
        }
    })

    // State for modal visibility and editing
    const showEditModal = ref(false); // Modal visibility state
    const editForm = ref({
    id: null,
    name: "",
    price: "",
    }); // Form state for editing

    const emit = defineEmits([
        'deleteTransaction', 'editTransaction'
    ])
    
    const deleteTransaction = (id) => {
        emit('deleteTransaction', id)
    }

    const showForm = (transaction) => {
        editForm.value = { ...transaction };
        showEditModal.value = true;
    };

    const closeModal = () => {
        showEditModal.value = false;
    };

    const editTransaction = () => {
        emit("editTransaction", { ...editForm.value });
        closeModal();
    };


</script>

<template>
    <h3>History</h3>
    <ul id="list" class="list">
        <li v-for="transaction in transactions"
            key="transaction.id"
            :class="transaction.price < 0 ? 'minus' : 'plus'">
            {{ transaction.name }} 
            <span>${{ transaction.price }}</span>
            <button @click="deleteTransaction(transaction.id)" class="delete-btn">🗑️</button>
            <button @click="showForm(transaction)" class="edit-btn">✍️</button>
            
        </li>
    </ul>

    <!-- Edit Modal -->
    <teleport to="body">
        <div v-if="showEditModal" class="modal-overlay" @click="closeModal">
            <div class="modal-content" @click.stop>
                <h3 style="margin: 10px 0;">Edit Transaction</h3>
                <form @submit.prevent="editTransaction">
                        <label for="name">Text</label>
                        <input type="text" id="name" v-model="editForm.name" required />
                        <label for="price">Amount
                            (negative - expense, positive - income)</label>
                        <input type="number" id="price" v-model="editForm.price" required />
                        <button type="submit" class="btn">Save</button>
                        <button type="button" class="btn" @click="closeModal">Cancel</button>
                </form>
            </div>
        </div>
    </teleport>

</template>

<style>
    .modal-overlay {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: rgba(0, 0, 0, 0.5);
        display: flex;
        justify-content: center;
        align-items: center;
        z-index: 1000;
    }

    .modal-content {
        background-color: #fff;
        padding: 20px;
        width: 400px;
        max-width: 90%;
    }
</style>