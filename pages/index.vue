<script  setup>
import { ref } from 'vue';
const name = ref('')
const city = ref('')
const dob = ref('')

const loader = ref(false)
const isError = ref(false)
const isSucess = ref(false)

const deleteLoader = ref(false)


const { data:contacts, pending, error, refresh } = await useFetch('http://localhost:4000/contacts',)

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
        refresh()
    } catch (error) {
        isError.value = true 
        loader.value = false
    }
    
}

async function  deleteConact(id) {
    try {
        deleteLoader.value = true
        const response = await fetch(`http://localhost:4000/contacts/${id}`, {
            method: "DELETE"
        })
        deleteLoader.value = false
        refresh()
    } catch (error) {
        
    }
    
}

</script>
<template >
    <div>
        <div v-if="isError"> error</div>
        <div v-if="loader">loading...</div>
       <label for=""> nom </label> <input v-model="name" type="text" name="" id="">
       <label for=""> ville </label> <input v-model="city" type="text" name="" id="">
       <label > date de naissance </label> <input v-model="dob" type="datetime-local" name="" id="">
       <button @click="submitForm"> envoyer  </button>
    </div>

    <div>
        <div v-if="deleteLoader">loading...</div>
        <ul>
            <li v-for="(contact,i ) in contacts">
                {{ i+1 }}
                <strong> nom:</strong>  {{  contact.name }} ,
                <strong> ville:</strong> {{  contact.city }} ,
                <strong> date de naissance :</strong> {{  new Date(contact.dob).toLocaleDateString() }} ,
                <button @click="deleteConact(contact._id)" style="background-color: red;"> delete  </button>
            </li>
        </ul>
    </div>
</template>
<style >
    
</style>