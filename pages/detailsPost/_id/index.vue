<template>
  <div>
      
      <div class="container">
       <DetailComponent   
        :text="posts.text"
        :image="posts.image"
        :picture="owner.picture"
        :firstName="owner.firstName"
        :lastName="owner.lastName"
        :publishDate="posts.publishDate"
        :tags="posts.tags"
        :likes="posts.likes"
        />
       <div class="comment-container">
        <CommentComponent/>
        <AddCommentComponent
        :post="posts.post"
        :id="posts.id"
        :image="posts.image"
        :likes="posts.likes"
        :idOwner="owner.id"
        :picture="owner.picture"
        :firstName="owner.firstName"
        :lastName="owner.lastName"
        :title="owner.title"
        :text="posts.text"
        :publishDate="posts.publishDate"
        :tags="posts.tags"
        
        />
       </div>
        </div>
      
  </div>
</template>

<script>

import DetailComponent from '../../../components/DetailComponent.vue'
import CommentComponent from '../../../components/CommentComponent.vue'
import AddCommentComponent from '../../../components/AddCommentComponent.vue'
import axios from "axios";
    export default {
        
        components:{
            DetailComponent,
            CommentComponent,
            AddCommentComponent
        },  
        data() {
         return {
            posts: {},
            owner:{},
            comments:{}
            }
        },
        async created() {
            try {
            const res = await axios.get(
                `https://dummyapi.io/data/v1/post/${this.$route.params.id}`,
                {
                headers: {
                    "app-id": "627b956fb058dc4fa16fa1b9",
                },
                }
            );
            this.posts = res.data;
            this.owner=res.data.owner
            console.log(this.posts);
            } catch (err) {
            console.log(err);
            }
    
        },
   
      
}
</script>
    
<style lang="scss" scoped>
.container{
    display:flex;
}
.comment-container{
    display: flexbox;
}
</style>