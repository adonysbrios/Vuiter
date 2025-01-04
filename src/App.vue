<template>
  <div class="app-layout">
      <section>
        <div class="pages">
          <h1>Vuiter</h1><hr class="border-gray-900 my-8">
          <router-link to="/">Home</router-link>
          <router-link to="/about">About</router-link>
          <router-link v-if="logged==true" to="/settings">Settings</router-link>
          <router-link v-if="logged==false" to="/login">Login</router-link>
          <router-link v-if="logged==true" to="/logout">Log out</router-link>
        </div>
      </section>
      <div class="mx-auto max-w-xl">
        <nav class="max-w-xl flex justify-start border-l-2 border-r-2 mx-auto border-b-2 border-gray-900 rounded-none">
          <h1 class="text-white text-3xl font-extrabold">{{ routeName }}</h1>
        </nav>
        <router-view class="border-l-2 min-h-screen border-r-2 border-gray-900 rounded-none px-8"/>
      </div>
      <section>
      </section>
  </div>
  <!--
  <nav>
  </nav>-->
</template>

<script>
import PocketBase from 'pocketbase'
import router from './router';
export default{
  name: 'App',
  data(){
    return {
      logged: false,
      routeName: ''
    }
  },
  methods: {
    isLogged(){
      const pb = new PocketBase('https://basis-cabin.pockethost.io')
      if(pb.authStore.token){
        return true
      }else{
        return false
      }
    }
  },
  mounted(){
    setInterval(()=>{
      this.routeName = router.currentRoute.value.name
      this.logged = this.isLogged();
    }, 100)
  }
}
</script>

<style>
img {
  object-fit:cover;
}
* {
  border-radius: 8px;
  font-family: 'Segoe UI';
  text-align: start;
}

.app-layout{
  display: grid;
  grid-template-columns: 1fr 36rem 1fr;
}

#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #fff;
  background-color: #000;
  min-height: 100vh;
  padding-bottom: 10vh;
}

.pages, nav {
  padding: 30px;
}

.pages a {
  font-weight:400;
  color: #fff;
  display: block;
  font-size: 2rem;
  font-family:'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
}

.pages a.router-link-exact-active, .pages h1 {
  color: #0084ff;
}

.pages h1{
  font-weight: 700;
  font-size: 3rem;
}

</style>
