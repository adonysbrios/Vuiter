<template>
  <div class="w-[36rem]">
    <br>
    <h1 class="w-[20rem] text-gray-700 text-2xl text-center mx-auto mt-[50px]">You will be redirected in {{ restante }} seconds.</h1>
  </div>
</template>

<script>
import router from '@/router';
import PocketBase from 'pocketbase'
export default{
  name: 'AboutView',
  data(){
    return {
        restante: 5
    }
  },
  mounted(){
    const pb = new PocketBase('http://127.0.0.1:8090')
    pb.authStore.clear()
    setInterval(()=>{
        this.restante--;
    }, 1000)
    setTimeout(()=>{
        router.push({path: '/login'})
    }, 5000)
    }
}
</script>