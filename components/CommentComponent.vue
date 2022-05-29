<template>
    <div>
       <div class="card__footer" v-for="comment in comments" :key="comment.id">
            <div class="user">
              <img
                v-bind:src="comment.owner.picture"
                alt="user__image"
                class="user__image"
                height="50"
              />
              <div class="user__info">
                <h5>{{comment.owner.firstName}} {{comment.owner.lastName}}</h5>  
                <h6>{{comment.message}}</h6>  
                <small> {{ new Date(comment.publishDate).toLocaleString("en-US",{month: 'short', day: 'numeric', year: 'numeric', day: 'numeric'}) }}</small>
                <small> {{ new Date(comment.publishDate).toLocaleTimeString()}}</small> 
              </div>
            </div>
            
          </div>
    </div>
</template>

<script>
import axios from "axios";
export default {
    name: "CommentComponent", 
     data() {
         return {
            comments:{}
            }
        },
        async created() {
            try {
            const res = await axios.get(
                `https://dummyapi.io/data/v1/post/${this.$route.params.id}/comment`,
                {
                headers: {
                    "app-id": "627b956fb058dc4fa16fa1b9",
                },
                }
            );
            
            this.comments=res.data.data
            console.log("Commmentaires:",this.comments);
            } catch (err) {
            console.log(err);
            }
    
        },
      
    }
</script>

<style lang="scss" scoped>
.card__footer {
  display: flex;
  padding: 1rem;
  margin-top: auto;
  margin-left: 5rem;;
}

.user {
  display: flex;
  gap: 0.5rem;
}

.user__image {
  border-radius: 50%;
}

.user__info > small {
  color: #666;
}
.user__info >h6{
  color: rgb(42, 45, 48);
}
</style>