<template>
  <section>
    <div class="home">
      <CreatePost :logged="logged" />
    </div>
    <hr class="rounded-none mb-8 border-gray-900">
    <div class="posts" v-for="post in posts">
      <div class="p-4 mb-2">
        <div class="flex">
          <div>
            <img
              class="inline-block size-10 rounded-full ring-2 ring-[#000011]"
              :src="
                'https://basis-cabin.pockethost.io//api/files/' +
                post.expand.user.collectionId +
                '/' +
                post.expand.user.id +
                '/' +
                post.expand.user.avatar
              "
              alt="avatar"
            />
          </div>
          <div class="ml-4 flex flex-col items-start">
            <p class="font-semibold">{{ post.expand.user.name }}</p>
            <a
              class="text-gray-700 text-xs"
              :href="'/profile/' + post.expand.user.id"
              >View profile</a
            >
          </div>
        </div>
        <div class="flex items-start mt-4">
             <p class="text-white">{{ post.text }}</p>
          </div>
      </div>
      <div class="grid grid-gallery gap-2">
        <img class='rounded-none' v-for="image in post.images" :src="
                'https://basis-cabin.pockethost.io//api/files/' +
                post.collectionId +
                '/' +
                post.id +
                '/' +
                image
              " alt="">
      </div>
      <div class="mb-4 pt-4 flex justify-around gap-x-4 px-4">
        <button class="px-8"><img class="size-6" src="/like.png" alt=""></button>
        <button class="px-8"><img class="size-6" src="/comment.png" alt=""></button>
      </div>
      <hr class="rounded-none mb-8 border-gray-900">
    </div>
  </section>
</template>

<style>
.grid-gallery{
grid-template-columns: 1fr 1fr;
}
</style>

<script>
import CreatePost from "@/components/CreatePost.vue";
import PocketBase from "pocketbase";

export default {
  name: "HomeView",
  components: {
    CreatePost,
  },
  data() {
    return {
      posts:[],
    };
  },
  mounted() {
    const pb = new PocketBase("https://basis-cabin.pockethost.io/");
    const record = pb
      .collection("posts")
      .getList(1, 30, {
        sort: "-created",
        expand: "user",
      })
      .then((e) => {
        this.posts = e.items;
      })
      .catch((e) => {
        console.log(e);
      });
  },
};
</script>
