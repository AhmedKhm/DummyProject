<template>
    <div>
        <b-form-select  v-model="id" size="sm" style="width:15rem;" >
          <b-select-option  value=null> Please select a user... </b-select-option>
          <b-select-option v-for="user in users" :key="user.id"  :value="user.id"> {{user.firstName}} {{user.lastName}} </b-select-option>  
        </b-form-select>
       
    </div>
</template>

<script>
import axios from "axios";
    export default {
        name:"SingleSelectComponent",
        data() {
            return {
            users: [],
            id:null
            }
        },
        async created() {
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
        },
    }
</script>

<style lang="scss" scoped>

</style>