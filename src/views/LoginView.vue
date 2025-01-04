<template>
  
  <div class="w-[36rem]" v-if="logged == false">
    <br>
    <h1 class="text-5xl font-bold text-white mt-[60px] mb-2">Log in Vuiter</h1>
    <p class="text-gray-300 mt-1">You should log in the app before do some actions in Vuiter.</p>
    <div>
      <p class="mt-4 mb-[2px] font-[600] text-white">Email</p>

      <input type="text" @change="
          (e) => {
            email = e.target.value;
          }
        " 
      class="block rounded-none min-w-full grow py-1.5 px-4 text-base bg-gray-900 placeholder:text-gray-600 focus:outline focus:outline-0 sm:text-sm/6" placeholder="Enter your email">
      <p class="mt-4 mb-[2px] font-[600] text-white">Password</p>
      <input
        @change="
          (e) => {
            password = e.target.value;
          }
        "
        type="password"
        class="block rounded-none min-w-full grow py-1.5 px-4 text-base bg-gray-900 placeholder:text-gray-600 focus:outline focus:outline-0 sm:text-sm/6" placeholder="Enter your password"
      /><br />
      <button class="bg-blue-600 w-full block py-2 text-center font-bold text-xl rounded-none" @click="loginUser()">Log in</button>
    </div>
  </div>
  <div class="w-[36rem]" v-else>
    <br>
    <h1 class="w-[20rem] text-gray-700 text-2xl text-center mx-auto mt-[50px]">You will be redirected in {{ restante }} seconds.</h1>
  </div>
</template>

<script>
import router from "@/router";
import PocketBase from "pocketbase";
export default {
  name: "LoginView",
  data() {
    return {
      email: "",
      password: "",
      logged: false,
      restante: 5,
    };
  },
  methods: {
    redirect_home() {
      this.logged = true;
      setInterval(() => {
        this.restante--;
      }, 1000);
      setTimeout(() => {
        router.push({ path: "/" });
      }, 5000);
    },
    loginUser() {
      const pb = new PocketBase("http://127.0.0.1:8090");
      pb.collection("users")
        .authWithPassword(this.email, this.password)
        .then(() => this.redirect_home())
        .catch(e => {console.log(e)});;
    },
  },
  mounted() {
    const pb = new PocketBase("http://127.0.0.1:8090");
    if (pb.authStore.token) {
      this.redirect_home();
    }
  },
};
</script>
