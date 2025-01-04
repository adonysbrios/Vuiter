<template>
<br>
 <div v-if="logged && render" class="mb-8">
    <div class="flex gap-x-4">
      <img class="inline-block size-10 rounded-full ring-2 ring-[#000011]" :src="'https://basis-cabin.pockethost.io//api/files/'+user.collectionId+'/'+user.id+'/'+user.avatar" alt="avatar">
      <textarea placeholder="What's happening?" class="bg-[#000] block" @change="(e)=>{text=e.target.value}"></textarea>
    </div>
    <div class="flex justify-between mt-4">
      <input :files="files"  @change="e => files=e.target.files" type="file" hidden multiple id="images"><label for="images"><img class="size-6" src="/Image.png" alt=""></label>
      <button @click="sendPost()">Vueeet</button>
    </div>
 </div>
 <div class="p-10" v-if="!logged">
  <h1 class="text-[2rem] font-bold">Login</h1>
  <p class="text-gray-300">You should log in the app before create a public post</p>
  <router-link class="bg-blue-600 py-2 block text-center font-bold text-white rounded-full mt-4" to="/login">Login</router-link>
 </div>
</template>

<style scoped>  
/*img{
  height: 2.5rem;
  width: 3rem;
  border-radius: 360px;
}*/
  textarea, label, button{
    resize: none;
    box-sizing: border-box;
  }

  label{
    padding: 5px;
    font-size: 10px;
    border: solid 1px #0084ff;
    border-radius: 8px;
    margin-left: 3rem;
  }

  textarea{
    width: 90%;
    height: 80px;
    padding: 8px;
    color: #fff
  }

  button {
    background-color: #0084ff;
    color: #fff;
    border: solid 1px #0084ff;
    padding: 0 20px;
  }
</style>

<script>
import PocketBase from 'pocketbase'
export default {
  name: 'CreatePost',
  data(){
    return {
      logged: false,
      files: {},
      text: '',
      render: 'true',
      user: ''
    }
  },
  methods: {
    isLogged(){
      const pb = new PocketBase('https://basis-cabin.pockethost.io/')
      if(pb.authStore.token){
        this.user = pb.authStore.record
        return true
      }else{
        return false
      }
    },
    sendPost(){
      const pb = new PocketBase('https://basis-cabin.pockethost.io/');
      const formData = new FormData();
      formData.append('user', pb.authStore.record.id);
      formData.append('text', this.text);
      if(this.files.length){
        for(let file of this.files){
          formData.append('images', file);
        }
      }
      pb.collection('posts').create(formData).then(()=>{
        window.location.reload()
      }).catch(e => {console.log(e)});
    }
  },
  mounted(){
    setTimeout(()=>{
      this.logged = this.isLogged();
    }, 1)
  }
}

</script>
