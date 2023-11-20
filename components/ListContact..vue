<script setup>
const { data:contacts, pending, error, refresh } = await useFetch('http://localhost:4000/contacts',)
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
const deleteLoader = ref(false)

</script>
<template lang="">
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
<style lang="">
    
</style>