<template>
    <div class="container">
        <div class="row justify-content-center mt-3">
            <div class="col-md-6">
             <li class="list-group-item active text-center">Chat App</li>
            <ul class="list-group" v-chat-scroll >
            <div v-for="value in chat.message" :key="value.index">
             <li class="list-group-item list-group-item-success text-success" >{{ value }}
             </li>
             <small class="badge float-right badge-success">You</small>
            </div>

            </ul>
            <div class="form-group">
              <input type="text" class="form-control" placeholder="Type here..." name="message" v-model="message" @keyup.enter="save">
            </div>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        data(){
            return{
                user:[],
                message:'',
                chat:{
                    message:[]
                },

            }

        },

        mounted() {
            Echo.private('Apidp.User'+ this.user.id)
            .listen('ChatEvent', (e) => {
                console.log(e);
            });
        },
        created(){
            this.getUser()

        },
        methods:{
            save(){
                if(this.message.length!=0)
                {
                    this.chat.message.push(this.message)
                    this.message=''

                }

            },
            getUser(){
                this.axios.get('api/send')
                .then((response)=>{
                    this.user=response.data

                })
            }
        }

    }
</script>
<style scoped>
.list-group{
    overflow-y: scroll;
    height: 200px;
}
</style>
