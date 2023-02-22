<script setup>
import axios from 'axios'
import { reactive, onMounted } from 'vue';
let data = reactive({
    title: "Hello World",
    contentList: []
})
// let title = ref("Hello World")
// let contentList = ref([])

onMounted(async ()=>{
    let response;
    try{
        response = await axios.get('api/posts/')
    } catch(error) {
        response = error.response
    }

    if (response.status === 200) {
        let responseData = response.data
        data.title = "Post"
        data.contentList = responseData.data
    } else {
        data.title = "Not Found"
    }


    // let responseData = await fetch('api/posts/').then(res=>{
    //     if (res.status === 200) {
    //         return res.json()
    //     } else {
    //         return "Not Found"
    //     }
    // })
    // if (responseData instanceof String || typeof(responseData) === "string") {
    //     data.title = responseData
    // } else {
    //     data.title = "Post"
    //     data.contentList = responseData.data
    // }
})

</script>

<template>
    <div>
        <h1>
            {{ data.title }}
        </h1>
        <div v-for="post in data.contentList" :key="post.id">
            {{ post.id }} - {{ post.title }}
        </div>
    </div>
</template>