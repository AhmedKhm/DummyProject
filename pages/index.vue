<template>
  <div >
       <div>
        <b-form-select  v-model="idUser" size="sm" style="width:15rem;" @change="DisplayUser">
          <b-select-option  value=null> Please select a user... </b-select-option>
          <b-select-option value=1> <strong> <---- Show All posts ----> </strong></b-select-option>
          <b-select-option v-for="user in users" :key="user.id"  :value="user.id" > {{user.firstName}} {{user.lastName}} </b-select-option>  
        </b-form-select>
        </div>
    <div class="wrapper">    
     <div  class="list-container"> 
      <PostComponent  
        id="items"
        v-for="post in posts" :key="post.id"
        :text="post.text"
        :image="post.image"
        :owner="post.owner"
        :publishDate="post.publishDate"
        :tags="post.tags"
        :likes="post.likes"
        :idPost="post.id"
        />
     </div> 
      <div class="mt-3">
        <b-pagination
          aria-controls="items"
          v-model="currentPage"
          :total-rows="totalPosts"
          :per-page="perPage"
          align="fill"
         @change="onChange"
        ></b-pagination>
         
      </div> 

    </div> 
  </div>
</template>

<script>
import axios from "axios";
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
      currentPage : 0,
      perPage : 12,
      users: [],
      userPosts:[],
      totalPosts:0,
      users: [],
      idUser:"",
    }
  },
   mounted(){
     this.getPosts(this.currentPage)
     this.getListUsers()
   },
   created() {   
  },
    methods:{

       //Returning the list of Posts
      getPosts(param){
         const res =  axios.get(
        `https://dummyapi.io/data/v1/post?page=${param}&limit=${this.perPage}`,
        {
          headers: {
            "app-id": "627b956fb058dc4fa16fa1b9",
          },
        }
      ).then((res)=>{
        this.posts = res.data.data;
        this.totalPosts=res.data.total;
        this.owner=res.data.data.owner
        console.log(res.data);
      }).catch((error)=>{
        console.error(error);
      })
     
      },
      //returning the list of users to display in the select box
      getListUsers(){
         const res =  axios.get(
            `https://dummyapi.io/data/v1/user`,
            {
            headers: {
                "app-id": "627b956fb058dc4fa16fa1b9",
            },
            }
            ).then((res)=>{
            this.users = res.data.data;
            }).catch((error)=>{
             console.error(error);
      })  
      },
       //returning the list of posts specific to a user
        getPostsByUser(param){
            const res =  axios.get(
            `https://dummyapi.io/data/v1/user/${param}/post`,
            {
            headers: {
                "app-id": "627b956fb058dc4fa16fa1b9",
            },
            }
            ).then((res)=>{
            this.posts = res.data.data;
            console.log("posts of user are here",this.posts)
            }).catch((error)=>{
             console.error(error);
      }) 
        },  
      // listening to the state of currentPage and returning the page clicked in pagination
    onChange() {
      console.log("page:",this.currentPage)
      this.getPosts(this.currentPage)
    },
    DisplayUser(){
      if (this.idUser==1){
        this.getPosts()
      }else{
          this.getPostsByUser(this.idUser)
      }
      
    }
    },

  computed:{
       
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
.wrapper{
  display: flexbox;
}
</style>