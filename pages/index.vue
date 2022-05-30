<template>
  <div class="container">
      <div>
        <b-form-select  v-model="idUser" size="sm" style="width:15rem;" >
          <b-select-option  value=null> Please select a user... </b-select-option>
          <b-select-option v-for="user in users" :key="user.id"  :value="user.id" > {{user.firstName}} {{user.lastName}} </b-select-option>  
        </b-form-select>
       
    </div>
    
     <div  class="list-container"> 
      <PostComponent  
        v-for="post in posts" :key="post.id"
        :text="post.text"
        :image="post.image"
        :owner="post.owner"
        :publishDate="post.publishDate"
        :tags="post.tags"
        :likes="post.likes"
        :id="post.id"
        />
   
      <div class="mt-3">
        <b-pagination
          aria-controls="post-component"
          v-model="currentPage"
          :total-rows="rows"
          :per-page="perPage"
          align="fill"
         
        ></b-pagination>
      </div> 
    </div> 
    
  </div>
</template>

<script>
import axios from "axios";
import {state} from 'vuex'
import PostComponent from "../components/PostComponent.vue";
import SingleSelectComponent from"../components/SingleSelectComponent.vue";
export default {
  components: {
    PostComponent,
    SingleSelectComponent
  },
  data() {
    return {
      posts: [],
      currentPage : 1,
      perPage : 3,
      users: [],
      userPosts:[],
      idUser:"",
      
    }
  },
//Returning the list of Posts
  async created() {
    try {
      const res = await axios.get(
        `https://dummyapi.io/data/v1/post?page=2&limit=12`,
        {
          headers: {
            "app-id": "627b956fb058dc4fa16fa1b9",
          },
        }
      );
      this.posts = res.data.data;
      this.owner=res.data.data.owner
      console.log(res);
    } catch (err) {
      console.log(err);
    }
//Returning the list of users in the select box
    try {
        const res = await axios.get(
            `https://dummyapi.io/data/v1/user`,
            {
            headers: {
                "app-id": "627b956fb058dc4fa16fa1b9",
            },
            }
        );
        this.users = res.data.data;
        console.log("Users list:",this.users);
        } catch (err) {
        console.log(err);
        }
// returning the list of posts specific to a user
       try {
         let param=this.idUser
        const res = await axios.get(
            `https://dummyapi.io/data/v1/${{param}}/post`,
            {
            headers: {
                "app-id": "627b956fb058dc4fa16fa1b9",
            },
            }
        );
        this.userPosts = res.data.data;
        console.log("User Posts list:",this.userPosts);
        } catch (err) {
        console.log(err);
        }
  },
  computed:{
       getUserId(){
         return this.idUser
       },
       itemsForList() 
       {
        return this.posts?.slice(
            (this.currentPage - 1) * this.perPage,
            this.currentPage * this.perPage,
         );
       },
       rows(){
           return this.posts.length;
       },
       
  }
};
</script>

<style lang="scss" scoped>
.container{
  display: flex;
  margin-left: 5rem;
}
.list-container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-gap: 10px;
  grid-auto-rows: minmax(100px, auto);
}
</style>