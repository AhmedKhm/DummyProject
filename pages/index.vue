<template>
  <div class="container">
      <SingleSelectComponent/>
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
      
    }
  },
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
  },
  computed:{
       itemsForList() 
       {
        return this.posts?.slice(
            (this.currentPage - 1) * this.perPage,
            this.currentPage * this.perPage,
         );
       },
       rows(){
           return this.posts.length;
       }
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