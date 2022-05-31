<template>
    <div>
      <div class="container">
        <div class="card-footer py-3 border-0" style="background-color: #f8f9fa;">
            <div class="d-flex flex-start w-100">
              <img class="rounded-circle shadow-1-strong me-3"
                v-bind:src="picture" alt="avatar" width="40"
                height="40" />
              <div class="form-outline w-100">
                <textarea class="form-control" id="textAreaExample" rows="2" placeholder="Type your comment here..." v-model="message"
                  style="background: #fff;"></textarea>
              </div>
            </div>
            <div class="float-end mt-2 pt-1">
              <button type="button" v-on:click="commentSubmit" class="btn btn-primary btn-sm">
                Post comment
              </button>
            </div>
          </div>
        </div>  
    </div>
</template>

<script>
import axios from "axios";
    export default {
        name:"AddCommentComponent",
        props: [ "post","id", "owner", "image", "text", "publishDate","tags","likes","picture","idOwner","firstName","lastName","title"],
        data() {
         return {
            message:""
            }
        },
        methods: {
              commentSubmit() {
              
              axios.post('https://dummyapi.io/data/v1/comment/create', {
              message:this.message,
              owner: this.idOwner,
              post: this.id
              
              },
               {
                headers: {
                    "app-id": "627b956fb058dc4fa16fa1b9",
                },
               }
              
              )
              .then(function (response) {
              })
              .catch(function (error) {
              });
              }
      }
    }

</script>

<style lang="scss" scoped>
.container{
  margin-left:4rem;
}
</style>