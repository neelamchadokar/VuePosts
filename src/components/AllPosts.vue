<script>
import axios from 'axios'
export default {
name:'AllPosts',
data(){
  return{
    allPosts:[],
    post:'hello dear',
    comments:[],
    showId:null
  }
},
computed:{
  count(){
    return this.allPosts.length
  }
},
methods: {
  getPosts(){
    return new Promise((res,rej)=>{
      const req ={
        method:"GET",
        url:"https://dummyjson.com/posts"
      }
      axios(req)
      .then((res)=>{
        console.log(res)
        this.allPosts=res.data.posts
      })
      .catch(err=>console.log(err))
    })
  },
  getComments(id){
    this.comments=[]
    this.showId=id
    return new Promise((res,rej)=>{
      const req ={
        method:"GET",
        url:`https://dummyjson.com/posts/${id}/comments`
      }
      axios(req)
      .then((res)=>{
        console.log(res)
        this.comments=res.data.comments
      })
      .catch(err=>console.log(err))
    })
  }
},
created(){
  this.getPosts()
}
}
</script>
<template>

<h1 class="bg-pink-300 block text-center text-black h-8 font-semibold align-middle">All Posts</h1>
<div class="bg-red-300 flex align-middle justify-center h-48" v-if="count<1">...Loading</div>
<table class="p-2" v-else>
  <th>id</th>
  <th>user id</th>
  <th>title</th>
  <th>body</th>
  <th>action</th>
  <tr class="p-4  m-1 border-2 odd:bg-gray-200 odd:text-black even:bg-blue-800" v-for="post in allPosts" :key="post.id">
    <td class="w-[10%]">{{ post.id }}</td>
    <td class="w-[10%]">{{ post.userId }}</td>
    <td class="w-[30%]">{{ post.title }}</td>
    <td class="w-[30%]">{{ post.body }}</td>
    <td class="w-[20%] align-middle"><button v-if="showId!==post.id" class="bg-blue-200 rounded-sm m-2 text-inherit p-2" @click="getComments(post.id)"> 
    Show comments
    </button>
    <div v-else>
<ul>
  <li v-if="comments.length<1" class="text-red-300">No comments available</li>
  <li class="odd:bg-pink-300 p-2 text-center" v-for="com in comments" :key="com.id">{{ com.body }}</li>
</ul>
    </div>
    </td>
  </tr>
</table>
</template>

<style lang="css" src="./AllPost.css">
.read-the-docs {
  color: #888;
}
</style>
