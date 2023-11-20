<script setup>
import { ref } from 'vue';
const name = ref('')
const city = ref('')
const dob = ref('')

async function  submitForm() {
    try {
        loader.value = true
        const response = await fetch('http://localhost:4000/contacts', {
            method: "POST",
            body:JSON.stringify({ name:name.value,
                city:city.value,
                dob:dob.value
            }),
            headers: {
            "Content-Type": "application/json",
            },
        })
        loader.value = false
        isSucess.value = true
    } catch (error) {
        isError.value = true 
        loader.value = false
    }
    
}
</script>
<template lang="">
     <div>
        <div v-if="isError"> error</div>
        <div v-if="loader">loading...</div>
       <label for=""> nom </label> <input v-model="name" type="text" name="" id="">
       <label for=""> ville </label> <input v-model="city" type="text" name="" id="">
       <label > date de naissance </label> <input v-model="dob" type="datetime-local" name="" id="">
       <button @click="submitForm"> envoyer  </button>
    </div>
</template>
<style lang="">
    
</style>