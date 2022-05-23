<template>
  <div class="home bg-slate-100 py-10">
    <div class="container mx-auto">
      <div
        class="bg-white p-3 mb-4 rounded-lg"
        v-for="(post, index) of friendsPost"
        :key="post.id"
      >
        <div class="flex items-center">
          <img class="w-12 h-12 rounded-full mr-4 object-cover" :src="post.info.avatar" alt="">
          <div>
            <p class="font-semibold">
              {{ post.info.name + " " + post.info.surname }}
            </p>
            <p>{{ post.createdAt }}</p>
          </div>
        </div>
        <p class="my-2">{{ post.text }}</p>
        <img class="w-full" :src="post.image" alt="">
        <p @click="setLike(index)">{{ post.likes.length }}</p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "HomeView",
  data() {
    return {
      users: null,
      posts: null,
      currentUser: localStorage.getItem("loggedUser"),
    };
  },
  computed: {
    activeUser() {
      if (this.users === null) {
        return console.log("...");
      } else {
        return this.users.filter((e) => e.email === this.currentUser);
      }
    },
    userPosts() {
      if (this.posts === null) {
        return console.log("...");
      } else {
        return this.posts.filter((e) => e.user_login === this.currentUser);
      }
    },
    userFriends() {
      return this.users.filter((i) =>
        this.activeUser[0].friends.includes(i.email)
      );
    },
    friendsPost() {
      if (this.posts) {
        let arr = this.posts.filter((e) =>
          this.activeUser[0].friends.includes(e.user_login)
        );
        let arrWithInfo = arr.map((post) => {
          post.info = this.userFriends.find(
            (user) => post.user_login == user.email
          );
          return post;
        });
        return arrWithInfo;
      }
      return [];
    },
  },
  async created() {
    let res = await axios.get(
      "https://6282500ded9edf7bd882691b.mockapi.io/users"
    );
    this.users = res.data;

    let posts = await axios.get(
      "https://6282500ded9edf7bd882691b.mockapi.io/posts"
    );
    this.posts = posts.data;
  },
  methods: {
    async setLike(id) {
      let postID = this.friendsPost[id].id;
      if (this.friendsPost[id].likes.includes(this.currentUser)) {
        let myIndex = this.friendsPost[id].likes.indexOf(this.currentUser);
        this.friendsPost[id].likes.splice(myIndex, 1);
        await axios.put(
          "https://6282500ded9edf7bd882691b.mockapi.io/posts/" + postID,
          this.friendsPost[id]
        );
      } else {
        console.log(postID);
        this.friendsPost[id].likes.push(this.currentUser);
        await axios.put(
          "https://6282500ded9edf7bd882691b.mockapi.io/posts/" + postID,
          this.friendsPost[id]
        );
      }
    },
  },
};
</script>
