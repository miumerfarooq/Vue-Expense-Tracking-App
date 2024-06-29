<template>
    <div>
        <h2 class="is-size-5 py-1 my-2 bb">Add New Transaction</h2>
        <form @submit.prevent="handleSubmit">
            <div class="field mb-1">
                <label class="label" for="text">Text</label>
                <div class="control">
                    <input type="text" v-model="title" @blur="titleTouched = true" class="input input__field" name="title" id="text" placeholder="Enter text">
                </div>
                <p v-if="!titleIsValid && titleTouched" class="help is-danger">Text is required</p>
            </div>
            <div class="field mb-4">
                <label class="label" for="amount">Amount <span class="is-size-7 mb-1">(Negative - expense, Positive - income)</span></label>
                <div class="control">
                    <input type="text" v-model="amount" @blur="amountTouched = true" class="input input__field" name="amount" id="amount" placeholder="Enter amount">
                </div>
                <p v-if="!amountIsValid && amountTouched" class="help is-danger">Amount is required and must be a valid number</p>
            </div>
            <div>
                <button type="submit" class="button is-success is-fullwidth">Add Transaction</button>
            </div>
        </form>
    </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import { useToast } from "vue-toastification"

const title = ref('')
const amount = ref(0)
const titleTouched = ref(false)
const amountTouched = ref(false)
const toast = useToast()

const emit = defineEmits(['transactionSubmitted'])

const titleIsValid = computed(() => !!title.value.trim())
const amountIsValid = computed(() => !isNaN(parseFloat(amount.value)) && !!amount.value)

const handleSubmit = (event) => {
    titleTouched.value = true
    amountTouched.value = true
    if (!titleIsValid.value ||!amountIsValid.value) {
        // console.log('All fields are required and amount must be a valid number')
        toast.error('All fields are required');
        return
    }
    const transactionData = {
        title: title.value, amount: parseFloat(amount.value)
    }
    emit('transactionSubmitted', transactionData)
    // console.log(`title = ${title.value} & amount = ${amount.value}`)
    title.value = ''
    amount.value = 0
    titleTouched.value = false
    amountTouched.value = false
}
</script>

<style>
.input__field {
    background-color: #00d1b2 !important;
    border: none !important;
    border-radius: 0 !important;
}
</style>
