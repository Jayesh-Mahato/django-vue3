<script setup>
import axios from 'axios'
import store from './../store'
import {ref, reactive} from 'vue'
const initFormData = {
    'title': '',
    'slug': '',
    'content':'',
}
const formData = reactive({...initFormData})
const error = ref({})
const handleAutoSlugChange = (event) => {
    // console.log(formData.title)
    // console.log(event.target.value)
    // formData.slug = slugify(event.target.value)
    formData.slug = event.target.value
}
const handleSubmitForm = async (event) => {
    if (event){
        event.preventDefault()
    }
    const target = event.target
    const innerFormData = new FormData(target)
    const innerFormDataJson = JSON.stringify(Object.fromEntries(innerFormData))
    const formDataJson = JSON.stringify(formData)
    // console.log(innerFormDataJson, formDataJson)
    const csrfToken = store.token
    const axiosConfig = {headers: {"X-CSRFToken": csrfToken}}
    let response;
    try {
        response = await axios.post('/api/posts/create',
        formDataJson, axiosConfig)
    } catch (err) {
        response = err.response
        error.value = err.response
        
    }

    if (response.status === 201) {
        for (let key of Object.keys(formData)) {
            formData[key] = initFormData[key]
        }
    }
    if (response.status === 500) {
        alert("Server failed; Please try again.")
    }
    console.log(error.value)
    console.log(response)
}

//let title = ref('')
</script>

<template>
    <br/><h2>CreateForm.vue</h2>
    <form method="POST" @submit.prevent="handleSubmitForm">
        <div>
            <input type="text" required v-model="formData.title" name="title" placeholder="Your Blog Title" 
            v-on:keyup="handleAutoSlugChange"
            />
        </div>
        <div>
            <input type="text" required v-model="formData.slug" name="slug" placeholder="your-blog-slug" />
        </div>
        <div>
            <textarea name="content" v-model="formData.content"></textarea>
        </div>
        <div>
            <p>Preview:</p>
            <p>Preview: {{ formData.title }}</p>
            <p>Preview: {{ formData.slug }}</p>
            <button type="submit">Send</button>
            <!-- <button @click="handleSubmitForm">Send</button> -->
        </div>
    </form>
</template>